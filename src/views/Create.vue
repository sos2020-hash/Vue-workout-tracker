<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!-- Status Message -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 bg-light-grey rounded-md"
    >
      <p class="text-at-light-green">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>
    <!-- Create -->
    <div class="p-8 flex items-start bg-light-grey rounded-md shadow-lg">
      <!-- Form -->
      <form
        @submit.prevent="createWorkout"
        class="flex flex-col gap-y-5 w-full"
      >
        <h1 class="text-2xl text-al-light-green">Record Workout</h1>
        <!-- Workout Name -->
        <div class="flex flex-col">
          <label for="workout-name" class="mb-1 text-sm text-at-ligth-green"
            >Workout Name</label
          >
          <input
            type="text"
            required
            class="p-2 text-gray-500 focus:outline-none"
            id="workout-name"
            v-model="workoutName"
          />
        </div>
        <!-- Workout Type -->
        <div class="flex flex-col">
          <label for="workout-type" class="mb-1 text-sm text-at-ligth-green"
            >Workout Type</label
          >
          <select
            required
            @change="workoutChange"
            class="p-2 text-gray-500 focus:outline-none"
            id="workout-type"
            v-model="workoutType"
          >
            <option value="select-workout">Select Workout</option>
            <option value="strength">Strength</option>
            <option value="cardio">Cardio</option>
          </select>
        </div>
        <!-- Strength Training Inputs -->
        <div class="flex flex-col gap-y-4" v-if="workoutType === 'strength'">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label
                for="exercise-name"
                class="mb-1 text-sm text-at-light-green"
                >Exercise</label
              >
              <input
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.exercise"
                required
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="sets" class="mb-1 text-sm text-at-light-green"
                >Sets</label
              >
              <input
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.sets"
                required
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="reps" class="mb-1 text-sm text-at-light-green"
                >Reps</label
              >
              <input
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.reps"
                required
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="weight" class="mb-1 text-sm text-at-light-green"
                >Weight</label
              >
              <input
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.weight"
                required
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light-green.png"
              class="h-4 w-auto absolute -left-4 cursor-pointer"
              alt=""
            />
            <button
              @click="addExercise"
              type="button"
              class="mt-6 py-2 px-6 rounded-sm self-start text-sm
      text-white bg-at-light-green duration-200 border-solid 
      border-2 border-transparent hover:border-at-light-green hover:bg-white
      hover:text-at-light-green
      "
            >
              Add Exercise
            </button>
          </div>
        </div>
        <!-- Cardio Inputs -->
        <div class="flex flex-col gap-y-4" v-if="workoutType === 'cardio'">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label for="cardio-type" class="mb-1 text-sm text-at-light-green"
                >Type</label
              >
              <select
                id="cardio-type"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.cardioType"
              >
                <option value="#">Select Type</option>
                <option value="run">Runs</option>
                <option value="walk">Walk</option>
              </select>
            </div>
            <div class="flex flex-col flex-1">
              <label for="distance" class="mb-1 text-sm text-at-light-green"
                >Distance</label
              >
              <input
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.distance"
                required
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="duration" class="mb-1 text-sm text-at-light-green"
                >Duration</label
              >
              <input
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.duration"
                required
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="pace" class="mb-1 text-sm text-at-light-green"
                >Pace</label
              >
              <input
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.pace"
                required
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light-green.png"
              class="h-4 w-auto absolute -left-4 cursor-pointer"
              alt=""
            />
            <button
              @click="addExercise"
              type="button"
              class="mt-6 py-2 px-6 rounded-sm self-start text-sm
      text-white bg-at-light-green duration-200 border-solid 
      border-2 border-transparent hover:border-at-light-green hover:bg-white
      hover:text-at-light-green
      "
            >
              Add Exercise
            </button>
          </div>
        </div>
        <button
          type="submit"
          class="mt-6 py-2 px-6 rounded-sm self-start text-sm
      text-white bg-at-light-green duration-200 border-solid 
      border-2 border-transparent hover:border-at-light-green hover:bg-white
      hover:text-at-light-green
      "
        >
          Record Workout
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { supabase } from "../supabase/init";
import { ref } from "vue";
import { uid } from "uid";
export default {
  name: "create",
  setup() {
    // Create data
    const workoutName = ref("");
    const workoutType = ref("select-workout");
    const exercises = ref([1]);
    const statusMsg = ref(null);
    const errorMsg = ref(null);
    // Add exercise
    const addExercise = () => {
      if (workoutType.value === "strength") {
        exercises.value.push({
          id: uid(),
          exercise: "",
          sets: "",
          reps: "",
          weight: "",
        });
        return;
      }
      exercises.value.push({
        id: uid(),
        cardipType: "",
        distance: "",
        duration: "",
        pace: "",
      });
    };
    // Delete exercise
    const deleteExercise = (id) => {
      if (exercises.value.length > 1) {
        exercises.value = exercises.value.filter(
          (exercise) => exercise.id !== id
        );
        return;
      }
      errorMsg.value =
        "Error: Cannot remove, need to at least have one exercise";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };
    // Listens for chaging of workout type input
    const workoutChange = () => {
      exercises.value = [];
      addExercise();
    };
    // Create workout
    const createWorkout = async () => {
      try {
        const { error } = await supabase.from("workout").insert([
          {
            workoutName: workoutName.value,
            workoutType: workoutType.value,
            exercises: exercises.value,
          },
        ]);
        if (error) throw error;
        statusMsg.value = "Success: Workout Created";
        workoutName.value = null;
        workoutType.value = "select-workout";
        exercises.value = [];
        setTimeout(() => {
          errorMsg.value = null;
        }, 5000);
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = null;
        }, 5000);
      }
    };
    return {
      workoutName,
      workoutType,
      exercises,
      statusMsg,
      errorMsg,
      addExercise,
      workoutChange,
      deleteExercise,
      createWorkout,
    };
  },
};
</script>
