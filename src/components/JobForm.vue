<script>
export default {
  data() {
    return {
      form: {
        firstName: '',
        lastName: '',
        phone: '',
        email: '',
        jobType: '',
        jobSource: '',
        jobDescription: '',
        address: '',
        city: '',
        state: '',
        zipCode: '',
        area: '',
        startDate: '',
        startTime: '',
        endTime: '',
        testSelect: 'option0'
      }
    };
  },
  methods: {
    handleSubmit() {
      const apiToken = '2f46ad3fdfd995ba671df03a475e610594c748e4';
      const url = `https://api.pipedrive.com/v1/deals?api_token=${apiToken}`;

      const data = {
        title: `${this.form.jobType} - ${this.form.jobSource}`,
        person_id: null,
        org_id: null,
        value: 0,
        currency: 'USD',
        user_id: null,
        add_time: new Date().toISOString(),
        stage_id: null,
        status: 'open',
        visible_to: '3',
        custom_fields: {
        }
      };

      fetch(url, {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${apiToken}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      })
          .then(response => response.json())
          .then(data => {
            console.log('Success:', data);
            alert('Сделка успешно создана');
          })
          .catch((error) => {
            console.error('Error:', error);
            alert('Ошибка при создании сделки');
          });
    }
  }
};
</script>

<template>
  <div class="form-container">
    <form @submit.prevent="handleSubmit">
      <div class="form-grid">
        <div class="form-section">
          <h3>Client details</h3>
          <div class="name-fields">
            <input v-model="form.firstName" placeholder="First name" required type="text">
            <input v-model="form.lastName" placeholder="Last name" required type="text">
          </div>
          <input v-model="form.phone" placeholder="Phone" required type="text">
          <input v-model="form.email" placeholder="Email (optional)" type="email">
        </div>
        <div class="form-section">
          <h3>Job details</h3>
          <div class="name-fields">
            <input v-model="form.jobType" placeholder="Job type" required type="text">
            <input v-model="form.jobSource" placeholder="Job source" required type="text">
          </div>
          <textarea v-model="form.jobDescription" placeholder="Job description (optional)"></textarea>
        </div>
        <div class="form-section">
          <h3>Service location</h3>
          <input v-model="form.address" placeholder="Address" required type="text">
          <input v-model="form.city" placeholder="City" required type="text">
          <input v-model="form.state" placeholder="State" required type="text">
          <div class="name-fields">
            <input v-model="form.zipCode" placeholder="Zip code" required type="text">
            <input v-model="form.area" placeholder="Area" type="text">
          </div>
        </div>
        <div class="form-section">
          <h3>Scheduled</h3>
          <input v-model="form.startDate" placeholder="Start date" required type="date">
          <div class="name-fields">
            <input v-model="form.startTime" placeholder="Start time" required type="time">
            <input v-model="form.endTime" placeholder="End time" required type="time">
          </div>
          <select v-model="form.testSelect">
            <option value="option0">Test select</option>
            <option value="option1">select 1</option>
            <option value="option2">select 2</option>
          </select>
        </div>
      </div>
      <div class="button-container">
        <button type="submit">Create job</button>
        <button disabled>Save info</button>
      </div>
    </form>
  </div>
</template>

<style scoped>
.form-container {
  display: flex;
  justify-content: center;
  padding: 20px;
  background-color: #f9f9f9;
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  width: 100%;
  max-width: 1000px;
}

.form-section {
  background-color: #ffffff;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h3 {
  margin-top: 0;
  margin-bottom: 15px;
  font-size: 1.25rem;
  color: #333;
}

.name-fields {
  display: flex;
  gap: 15px;
}

input[type="text"],
input[type="email"],
input[type="date"],
input[type="time"],
textarea,
select {
  width: 100%;
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
  box-sizing: border-box;
}

input[name="firstName"],
input[name="lastName"] {
  flex: 1;
}

.button-container {
  display: flex;
  justify-content: center;
  padding: 20px;
}

button[type="submit"] {
  display: inline-block;
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
  margin-right: 20px;
}

button[type="submit"]:hover {
  background-color: #0056b3;
}
</style>

