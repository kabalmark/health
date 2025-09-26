<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Healthcare - Appointment Booking</title>
  <style>
    * { box-sizing: border-box; }
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: #edd5e5;
      display: flex;
      justify-content: center;
      min-height: 100vh;
    }

    .container {
      background: #c5d3ed url('https://i.ibb.co/7GQk7sB/medical-watermark.png') no-repeat center;
      background-size: 200px 200px; /* adjust size */
      margin: 20px;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
      width: 100%;
      max-width: 480px;
      position: relative;
    }

    h2, h3 { text-align: center; color: #2c3e50; }

    label {
      display: block;
      margin: 10px 0 5px;
      font-weight: bold;
      color: #34495e;
    }

    input, select {
      width: 100%;
      padding: 12px;
      margin-bottom: 12px;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 16px;
      background: rgba(255, 255, 255, 0.9); /* make input readable over watermark */
    }

    button {
      padding: 12px;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      cursor: pointer;
      transition: all 0.2s ease;
    }

    #submitBtn {
      width: 100%;
      background: #27ae60;
      color: #fff;
      font-weight: bold;
      margin-bottom: 15px;
    }
    #submitBtn:hover { background: #219150; }

    .appointments { margin-top: 20px; max-height: 300px; overflow-y: auto; }

    .appointment {
      background: rgba(236, 240, 241, 0.9);
      padding: 12px;
      margin-bottom: 12px;
      border-radius: 8px;
      font-size: 15px;
    }
    .appointment strong { color: #2c3e50; }

    .actions {
      margin-top: 8px;
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }
    .edit-btn { background: #3498db; color: #fff; flex:1; }
    .edit-btn:hover { background: #2980b9; }
    .delete-btn { background: #e74c3c; color: #fff; flex:1; }
    .delete-btn:hover { background: #c0392b; }

    #search {
      margin-bottom: 15px;
      padding: 12px;
      border: 1px solid #ccc;
      border-radius: 8px;
      width: 100%;
      font-size: 16px;
      background: rgba(255,255,255,0.9);
    }

    /* Responsive for mobile */
    @media(max-width: 500px){
      .appointment { font-size: 14px; }
      button { font-size: 15px; padding: 10px; }
      input, select { font-size: 15px; padding: 10px; }
      .container { background-size: 150px 150px; }
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Healthcare Appointment</h2>

    <!-- Search bar -->
    <input type="text" id="search" placeholder="Search by Name, Date, or Doctor">

    <form id="appointmentForm">
      <label>Full Name</label>
      <input type="text" id="name" required>

      <label>Age</label>
      <input type="number" id="age" required>

      <label>Contact</label>
      <input type="tel" id="contact" required>

      <label>Date</label>
      <input type="date" id="date" required>

      <label>Doctor</label>
      <select id="doctor" required>
        <option value="">-- Select Doctor --</option>
        <option>Dr. John Smith - Cardiologist</option>
        <option>Dr. Sarah Lee - Pediatrician</option>
        <option>Dr. Ahmed Khan - Dermatologist</option>
      </select>

      <button type="submit" id="submitBtn">Book Appointment</button>
    </form>

    <div class="appointments">
      <h3>My Appointments</h3>
      <div id="appointmentsList"></div>
    </div>
  </div>

  <script>
    const form = document.getElementById('appointmentForm');
    const list = document.getElementById('appointmentsList');
    const submitBtn = document.getElementById('submitBtn');
    const searchInput = document.getElementById('search');

    let appointments = JSON.parse(localStorage.getItem('appointments')) || [];
    let editIndex = null;

    function showAppointments(filtered = null) {
      const data = filtered || appointments;
      list.innerHTML = '';
      data.forEach((appt, index) => {
        const div = document.createElement('div');
        div.className = 'appointment';
        div.innerHTML = `
          <strong>${appt.name}</strong> (${appt.age} yrs)<br>
          Contact: ${appt.contact}<br>
          Date: ${appt.date}<br>
          Doctor: ${appt.doctor}
          <div class="actions">
            <button class="edit-btn" onclick="editAppointment(${index})">Edit</button>
            <button class="delete-btn" onclick="deleteAppointment(${index})">Delete</button>
          </div>
        `;
        list.appendChild(div);
      });
    }

    form.addEventListener('submit', function(e) {
      e.preventDefault();
      const newAppt = {
        name: document.getElementById('name').value,
        age: document.getElementById('age').value,
        contact: document.getElementById('contact').value,
        date: document.getElementById('date').value,
        doctor: document.getElementById('doctor').value
      };

      if (editIndex === null) {
        appointments.push(newAppt);
      } else {
        appointments[editIndex] = newAppt;
        editIndex = null;
        submitBtn.textContent = "Book Appointment";
      }

      localStorage.setItem('appointments', JSON.stringify(appointments));
      showAppointments();
      form.reset();
    });

    function deleteAppointment(index) {
      appointments.splice(index, 1);
      localStorage.setItem('appointments', JSON.stringify(appointments));
      showAppointments();
    }

    function editAppointment(index) {
      const appt = appointments[index];
      document.getElementById('name').value = appt.name;
      document.getElementById('age').value = appt.age;
      document.getElementById('contact').value = appt.contact;
      document.getElementById('date').value = appt.date;
      document.getElementById('doctor').value = appt.doctor;
      editIndex = index;
      submitBtn.textContent = "Update Appointment";
    }

    searchInput.addEventListener('input', function() {
      const query = this.value.toLowerCase();
      const filtered = appointments.filter(appt => {
        return appt.name.toLowerCase().includes(query) ||
               appt.date.includes(query) ||
               appt.doctor.toLowerCase().includes(query);
      });
      showAppointments(filtered);
    });

    showAppointments();
  </script>
</body>
</html>
