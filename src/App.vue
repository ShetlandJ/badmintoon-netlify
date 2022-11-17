<script setup>
import { ref, computed } from "@vue/runtime-core";
import Court from "./components/Court.vue";

const courts = ref([{ id: 1, players: [] }]);

const addNewCourt = () => {
    courts.value.push({ id: courts.value.length + 1, players: [] });
};

const players = ref(
    [
        "Alex",
        "Anthony",
        "Arran",
        "Barbara",
        "Dan",
        "David",
        "Fran",
        "Ian B",
        "Ian F",
        "Iona",
        "James B",
        "James S",
        "Jon",
        "Robert",
        "Robyn",
        "Ross M",
        "Ruth",
        "Urmila",
    ].map((player, index) => ({
        name: player,
        id: index,
        attending: true,
    }))
);

const attendingPlayerCount = computed(() => {
    return attendingPlayers.value.length;
});

const attendingPlayers = computed(() => {
    return players.value.filter((player) => player.attending);
});

const toggleAttending = (player) => {
    const playerIndex = players.value.findIndex((p) => p.id === player.id);

    players.value[playerIndex].attending =
        !players.value[playerIndex].attending;
};

const unassignedPlayers = computed(() => {
    let assignedIds = [];

    courts.value.forEach((court) => {
        court.players.forEach((player) => {
            assignedIds.push(player.id);
        });
    });

    return attendingPlayers.value.filter(
        (player) => !assignedIds.includes(player.id)
    );
});

const randomiseCourt = (court) => {
    court.players = [];

    for (let i = 0; i < 4; i++) {
        const randomPlayer =
            unassignedPlayers.value[
                Math.floor(Math.random() * unassignedPlayers.value.length)
            ];

        if (randomPlayer) {
            court.players.push(randomPlayer);
        }
    }
};

const randomiseAttendees = (index = null) => {
    if (index !== null) {
        randomiseCourt(courts.value[index]);
    } else {
        courts.value.forEach((court) => randomiseCourt(court));
    }
};

const resetCourts = (index = null) => {
    if (index !== null) {
        courts.value[index].players = [];
    } else {
        courts.value.forEach((court) => (court.players = []));
    }
};

const newPlayerName = ref("");

const addNewPlayer = () => {
    players.value.push({
        name: newPlayerName.value,
        id: players.value.length,
        attending: true,
    });

    newPlayerName.value = "";
};
</script>

  <template>
    <div class="flex p-4">
        <div class="ml-2" style="width: 250px">
            <p class="text-xl mb-4"><b>Player list:</b></p>
            <div
                v-for="player in players"
                :key="`${player.id}-${
                    player.attending ? 'attending' : 'not-attending'
                }`"
                class="text-lg flex mb-1 mt-1"
            >
                <p v-if="player.attending">✅</p>
                <p v-else>⛔️</p>
                <label
                    class="ml-4 mr-4 pr-4"
                    @click="toggleAttending(player)"
                    :for="player.id"
                    >{{ player.name }}</label
                >
            </div>

            <div class="flex mt-4">
                <input
                    style="max-width: 150px;"
                    v-model="newPlayerName"
                    class="border border-gray-400 rounded-lg p-2 mr-2"
                    type="text"
                    placeholder="Add new player"
                />
                <button
                    @click="addNewPlayer"
                    class="
                        bg-blue-500
                        hover:bg-blue-700
                        text-white
                        font-bold
                        py-2
                        px-4
                        rounded
                    "
                >
                    Add
                </button>
            </div>

            <!-- <div class="flex justify-center mt-4"> -->
                <div class="flex justify-center mt-2 mb-2">
                    <button
                        @click="randomiseAttendees()"
                        class="
                            px-8
                            py-3
                            text-white
                            bg-blue-600
                            rounded
                            focus:outline-none
                            disabled:opacity-25
                            mr-2
                        "
                    >
                        Randomise all
                    </button>
                </div>
                <div class="flex justify-center mt-2">
                    <button
                        @click="resetCourts()"
                        class="
                            px-8
                            py-3
                            text-white
                            bg-red-600
                            rounded
                            focus:outline-none
                            disabled:opacity-25
                        "
                    >
                        Reset courts
                    </button>
                </div>
            <!-- </div> -->
        </div>
        <div class="justify-center flex bg-yellow-100 items-center">
            <div>
                <div
                    v-for="(court, index) in courts"
                    :key="`count-${court.id + 1}-${court.players.length}`"
                >
                    <Court
                        class="m-3"
                        :court-data="court"
                        :key="`${JSON.stringify(court.players)}-${court.id}`"
                    />
                    <div class="flex justify-center">
                        <button
                            class="
                                mb-0
                                mr-4
                                text-blue-500
                                font-bold
                                py-2
                                px-4
                                rounded
                            "
                            @click="randomiseAttendees(index)"
                        >
                            randomise
                        </button>
                        <button
                            class="
                                mb-0
                                mr-4
                                text-red-500
                                font-bold
                                py-2
                                px-4
                                rounded
                            "
                            @click="resetCourts(index)"
                        >
                            clear
                        </button>
                    </div>
                    <div
                        v-if="court.id === courts.length"
                        class="flex justify-center"
                    >
                        <button @click="addNewCourt">
                            + Add another court
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
