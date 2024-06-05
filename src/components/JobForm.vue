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
        person_id: null, // Логика для создания/поиска персоны и получения ID
        org_id: null, // Логика для создания/поиска организации и получения ID
        value: 0,
        currency: 'USD',
        user_id: null, // Логика для назначения пользователя
        add_time: new Date().toISOString(),
        stage_id: null, // Логика для определения ID стадии
        status: 'open',
        visible_to: '3',
        custom_fields: {
          // Добавьте ваши настраиваемые поля здесь
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
  <form @submit.prevent="handleSubmit">
    <div>
      <h3>Client details</h3>
      <input v-model="form.firstName" placeholder="First name" required type="text">
      <input v-model="form.lastName" placeholder="Last name" required type="text">
      <input v-model="form.phone" placeholder="Phone" required type="text">
      <input v-model="form.email" placeholder="Email (optional)" type="email">
    </div>
    <div>
      <h3>Job details</h3>
      <input v-model="form.jobType" placeholder="Job type" required type="text">
      <input v-model="form.jobSource" placeholder="Job source" required type="text">
      <textarea v-model="form.jobDescription" placeholder="Job description (optional)"></textarea>
    </div>
    <div>
      <h3>Service location</h3>
      <input v-model="form.address" placeholder="Address" required type="text">
      <input v-model="form.city"  placeholder="City" required type="text">
      <input v-model="form.state" placeholder="State" required type="text">
      <input v-model="form.zipCode" placeholder="Zip code" required type="text">
      <input v-model="form.area" placeholder="Area" type="text">
    </div>
    <div>
      <h3>Scheduled</h3>
      <label>Дата начала:</label>
      <input v-model="form.startDate" placeholder="Start date" required type="date">
      <label>Время начала:</label>
      <input v-model="form.startTime" placeholder="Start time" required type="time">
      <label>Время окончания:</label>
      <input v-model="form.endTime" placeholder="End time" required type="time">
      <select v-model="form.testSelect">
        <option value="option0">Test select</option>
        <option value="option1">select 1</option>
        <option value="option2">select 2</option>
      </select>
    </div>
    <button type="submit">Отправить</button>
  </form>
</template>


<style scoped>
</style>
