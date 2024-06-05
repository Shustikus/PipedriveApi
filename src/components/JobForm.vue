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
        testSelect: '26'
      }
    };
  },
  methods: {
    handleSubmit() {
      const apiToken = '2f46ad3fdfd995ba671df03a475e610594c748e4';
      const personUrl = `https://api.pipedrive.com/v1/persons?api_token=${apiToken}`;
      const dealUrl = `https://api.pipedrive.com/v1/deals?api_token=${apiToken}`;

      // Создание нового контакта (person)
      const personData = {
        name: `${this.form.firstName} ${this.form.lastName}`,
        phone: this.form.phone,
        email: this.form.email
      };

      fetch(personUrl, {
        method: 'POST',
        headers: {
          'Authorization': `Bearer ${apiToken}`,
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(personData)
      })
          .then(response => response.json())
          .then(personResponse => {
            if (personResponse.success) {
              const personId = personResponse.data.id;

              // Создание новой сделки (deal) с использованием идентификатора созданного контакта
              const dealData = {
                title: `Job #${personId}`,
                person_id: personId,
                org_id: null,
                value: 0,
                currency: 'USD',
                user_id: null,
                add_time: new Date().toISOString(),
                stage_id: null,
                status: 'open',
                visible_to: '3',
                '633f33ad16ee4a70e90308dfcb7132919ee5de15': this.form.address,
                'f4ddbbc73f7085c7bff0cf678fbf5d2bb375f2d7': this.form.jobType,
                '1660423f968422d824404424ca568a89408fac46': this.form.jobSource,
                '2caef1c49f52695692500eb5b65b135083c51afa': this.form.jobDescription,
                'd2f70e9b91a44dd9c613481395057d6b10dd8e36': this.form.city,
                'c26267a783b4c288c5b384d8c413c3af4685db0d': this.form.state,
                '41545937bbfbab8997aad6c592e4a4b70be7bb82': this.form.zipCode,
                '543ce0c690dad5c3f991afbdbc9faf739e0101df': this.form.area,
                '69f671897627e96e73eb75c3389f2246a84ab98a': this.form.startDate,
                'f8e1aa00356a896a47e68b51dba2175002cc2d07': this.form.startTime,
                '6602d9d3e59c3a5d3027c983ec1809fa8a28cbf2': this.form.endTime,
                'f6baa1210019357a7227f1a2d11e866edeaefa9d': this.form.testSelect,
              };

              fetch(dealUrl, {
                method: 'POST',
                headers: {
                  'Authorization': `Bearer ${apiToken}`,
                  'Content-Type': 'application/json'
                },
                body: JSON.stringify(dealData)
              })
                  .then(response => response.json())
                  .then(dealResponse => {
                    console.log('Success:', dealResponse);
                    alert('Сделка успешно создана');
                  })
                  .catch(error => {
                    console.error('Error:', error);
                    alert('Ошибка при создании сделки');
                  });
            } else {
              console.error('Error creating person:', personResponse);
              alert('Ошибка при создании контакта');
            }
          })
          .catch(error => {
            console.error('Error:', error);
            alert('Ошибка при создании контакта');
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
            <option value="26">Test select</option>
            <option value="27">select 1</option>
            <option value="28">select 2</option>
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

