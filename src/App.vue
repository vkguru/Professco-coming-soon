<template>
  <main>
    <nav>
      <img src="./assets/img/logo.png" alt="logo" />
    </nav>
    <section>
      <div class="coming__text">
        <h1>Pass your certification exams with ease</h1>
        <p>We are building a platform that  would help you pass your certification exam. We know how difficult it can be joggling a lot of things together and we are making this for you. Fill the form below to get notified when we launch.</p>
        <form class="form__group" @submit.prevent="submit">
          <input 
            type="email" 
            name="email"
            placeholder="Enter email address" 
            v-model="form.email" 
          />
          <button type="submit">notify me</button>

          <template v-if="v$.email.$errors.length">
            <span 
              v-for="error in v$.email.$errors" 
              :key="error.$uid"
            >
              {{ error.$message }}
            </span>
          </template>

          <span v-else>{{ form.success }}</span>
        </form>
      </div>
      <div class="coming__img">
        <img src="./assets/img/student-professco-picture.png" alt="students smiling" />
      </div>
    </section>
</main>
</template>

<script setup>
import { reactive, computed } from "vue";
import { useVuelidate } from "@vuelidate/core";
import { required, email, helpers } from "@vuelidate/validators";

const form =  reactive({
  email: "",
  error: [],
  success: ""
})

const rules = computed(() => {
    return {
        email: { 
            required: helpers.withMessage("Your email address is required", required), 
            email: helpers.withMessage("Enter a valid email address", email)
        }
    }
}) 

const v$ = useVuelidate(rules, form);

async function submit() {
  const result = await v$.value.$validate();
  if (!result) return;
  form.success = "Thanks for showing interest, we would let you know when we go live!"
}
</script>

<style scoped>
main {
  max-width: 90rem;
  width: 96%;
  height: 100%;
  margin: auto;
}

nav {
  padding-top: 40px;
}

section {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 30px;
}

.coming__text {
  max-width: 615px;
  width: 100%;
}

.coming__text h1 {
  font-size: 68px;
  margin-bottom: 15px;
}

.coming__text p {
  margin-bottom: 15px;
  font-size: 18px;
  line-height: 28px;
}

.form__group {
  position: relative;
  max-width: 485px;
  width: 100%;
}

.form__group span {
  font-size: 14px;
}

.form__group input[type="email"] {
  border: 1.069px solid #000;
  border-radius: 100px;
  width: 100%;
  padding: 20px;
  font-size: 15px;
  background: transparent;
}

button {
  position: absolute;
  top: 7px;
  right: 7px;
  padding: 15px;
  border-radius: 100px;
  background: #E65539;
  border: none;
  color: #fff;
  cursor: pointer;
}


@media screen and (max-width: 900px) {
  main {
    width: 90%;
  }

  section {
    flex-direction: column;
  }

  .coming__text {
    max-width: 100%;
    margin-top: 30px;
  }

  .coming__img {
    display: none;
  }

  .coming__text h1 {
    font-size: 45px;
  }

  .coming__text p {
    font-size: 16px;
  }
}
</style>
