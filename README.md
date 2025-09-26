<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>kabalCare - Appointment Booking System</title>
    <style>
        :root {
            --primary: #2D9CDB;
            --primary-dark: #1c5fb3;
            --secondary: #f8f9fa;
            --accent: #F2994A;
            --text: #333;
            --light-text: #6c757d;
            --border: #E0E0E0;
            --error: #dc3545;
            --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #4e90c2;
            color: var(#FFFFFF);
            line-height: 1.6;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            color: rgb(245, 240, 240);
            padding: 20px 0;
            box-shadow: var(--shadow);
            margin-bottom: 30px;
        }
        
        .header-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .logo i {
            font-size: 28px;
        }
        
        .logo h1 {
            font-size: 24px;
            font-weight: 600;
        }
        
        nav ul {
            display: flex;
            list-style: none;
            gap: 20px;
        }
        
        nav a {
            color: rgb(255, 253, 253);
            text-decoration: none;
            font-weight: 500;
            padding: 5px 10px;
            border-radius: 4px;
            transition: background 0.3s;
        }
        
        nav a:hover, nav a.active {
            background: rgba(235, 223, 223, 0.2);
        }
        
        .main-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
        }
        
        @media (max-width: 768px) {
            .main-content {
                grid-template-columns: 1fr;
            }
        }
        
        .card {
            background: #fefefe;
            border-radius: 8px;
            box-shadow: var(--shadow);
            padding: 25px;
            margin-bottom: 20px;
        }
        
        .card h2 {
            margin-bottom: 20px;
            color: var(--primary);
            border-bottom: 2px solid var(--border);
            padding-bottom: 10px;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 500;
        }
        
        input, select, textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid var(--border);
            border-radius: 4px;
            font-size: 16px;
            transition: border 0.3s;
        }
        
        input:focus, select:focus, textarea:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 2px rgba(42, 125, 225, 0.2);
        }
        
        button {
            background: var(--primary);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 500;
            transition: background 0.3s;
        }
        
        button:hover {
            background: var(--primary-dark);
        }
        
        .btn-success {
            background: var(--accent);
        }
        
        .btn-success:hover {
            background: #218838;
        }
        
        .appointment-list {
            max-height: 400px;
            overflow-y: auto;
        }
        
        .appointment-item {
            border-left: 4px solid var(--primary);
            padding: 15px;
            margin-bottom: 15px;
            background: var(--secondary);
            border-radius: 0 4px 4px 0;
        }
        
        .appointment-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
        }
        
        .appointment-doctor {
            font-weight: 600;
            color: var(--primary);
        }
        
        .appointment-date {
            color: var(--light-text);
            font-size: 14px;
        }
        
        .appointment-patient {
            margin-bottom: 5px;
        }
        
        .appointment-actions {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }
        
        .btn-small {
            padding: 5px 10px;
            font-size: 14px;
        }
        
        .btn-danger {
            background: var(--error);
        }
        
        .btn-danger:hover {
            background: #c82333;
        }
        
        .empty-state {
            text-align: center;
            padding: 30px;
            color: var(--light-text);
        }
        
        .empty-state i {
            font-size: 48px;
            margin-bottom: 15px;
            opacity: 0.5;
        }
        
        footer {
            text-align: center;
            margin-top: 40px;
            padding: 20px;
            color: var(--light-text);
            border-top: 1px solid var(--border);
        }
        
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: 4px;
            color: white;
            background: var(--accent);
            box-shadow: var(--shadow);
            transform: translateX(150%);
            transition: transform 0.3s;
            z-index: 1000;
        }
        
        .notification.show {
            transform: translateX(0);
        }
        
        .notification.error {
            background: var(--error);
        }
    </style>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <header>
        <div class="container">
            <div class="header-content">
                <div class="logo">
                    <i class="fas fa-stethoscope"></i>
                    <h1>kabalCare Appointment System</h1>
                </div>
                <nav>
                    <ul>
                        <li><a href="#" class="active">Book Appointment</a></li>
                        <li><a href="#">My Appointments</a></li>
                        <li><a href="#">Find a Doctor</a></li>
                        <li><a href="#">Contact</a></li>
                    </ul>
                </nav>
            </div>
        </div>
    </header>
    
    <div class="container">
        <div class="main-content">
            <div class="booking-form-section">
                <div class="card">
                    <h2><i class="fas fa-calendar-plus"></i> Book New Appointment</h2>
                    <form id="appointmentForm">
                        <div class="form-group">
                            <label for="patientName">Full Name</label>
                            <input type="text" id="patientName" placeholder="Enter your full name" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="patientEmail">Email Address</label>
                            <input type="email" id="patientEmail" placeholder="Enter your email" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="patientPhone">Phone Number</label>
                            <input type="tel" id="patientPhone" placeholder="Enter your phone number" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="appointmentDate">Preferred Date</label>
                            <input type="date" id="appointmentDate" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="appointmentTime">Preferred Time</label>
                            <input type="time" id="appointmentTime" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="doctorSelect">Select Doctor</label>
                            <select id="doctorSelect" required>
                                <option value="">Choose a doctor</option>
                                <option value="Dr. Sarah Johnson">Dr. Sarah Johnson - Cardiology</option>
                                <option value="Dr. Michael Chen">Dr. Michael Chen - Dermatology</option>
                                <option value="Dr. Emily Rodriguez">Dr. Emily Rodriguez - Pediatrics</option>
                                <option value="Dr. James Wilson">Dr. James Wilson - Orthopedics</option>
                                <option value="Dr. Lisa Thompson">Dr. Lisa Thompson - Neurology</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="appointmentReason">Reason for Visit</label>
                            <textarea id="appointmentReason" rows="3" placeholder="Briefly describe the reason for your appointment"></textarea>
                        </div>
                        
                        <button type="submit" class="btn-success">
                            <i class="fas fa-calendar-check"></i> Book Appointment
                        </button>
                    </form>
                </div>
            </div>
            
            <div class="appointments-section">
                <div class="card">
                    <h2><i class="fas fa-list-alt"></i> My Appointments</h2>
                    <div class="appointment-list" id="appointmentList">
                        <!-- Appointments will be dynamically added here -->
                    </div>
                </div>
            </div>
        </div>
    </div>
    
    <footer>
        <div class="container">
            <p>&copy; 2025 kabalCare Appointment System. All rights reserved.</p>
        </div>
    </footer>
    
    <div class="notification" id="notification">
        Appointment booked successfully!
    </div>

    <script>
        // DOM Elements
        const appointmentForm = document.getElementById('appointmentForm');
        const appointmentList = document.getElementById('appointmentList');
        const notification = document.getElementById('notification');
        
        // Initialize the application
        document.addEventListener('DOMContentLoaded', function() {
            loadAppointments();
            setMinDate();
        });
        
        // Set minimum date to today
        function setMinDate() {
            const today = new Date();
            const dd = String(today.getDate()).padStart(2, '0');
            const mm = String(today.getMonth() + 1).padStart(2, '0');
            const yyyy = today.getFullYear();
            
            const minDate = yyyy + '-' + mm + '-' + dd;
            document.getElementById('appointmentDate').setAttribute('min', minDate);
        }
        
        // Handle form submission
        appointmentForm.addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Get form values
            const patientName = document.getElementById('patientName').value;
            const patientEmail = document.getElementById('patientEmail').value;
            const patientPhone = document.getElementById('patientPhone').value;
            const appointmentDate = document.getElementById('appointmentDate').value;
            const appointmentTime = document.getElementById('appointmentTime').value;
            const doctorSelect = document.getElementById('doctorSelect').value;
            const appointmentReason = document.getElementById('appointmentReason').value;
            
            // Create appointment object
            const appointment = {
                id: Date.now(), // Simple unique ID
                patientName,
                patientEmail,
                patientPhone,
                appointmentDate,
                appointmentTime,
                doctor: doctorSelect,
                reason: appointmentReason,
                status: 'Scheduled'
            };
            
            // Save appointment
            saveAppointment(appointment);
            
            // Show success notification
            showNotification('Appointment booked successfully!', 'success');
            
            // Reset form
            appointmentForm.reset();
            setMinDate();
            
            // Reload appointments
            loadAppointments();
        });
        
        // Save appointment to localStorage
        function saveAppointment(appointment) {
            let appointments = getAppointments();
            appointments.push(appointment);
            localStorage.setItem('appointments', JSON.stringify(appointments));
        }
        
        // Get appointments from localStorage
        function getAppointments() {
            let appointments = localStorage.getItem('appointments');
            if (appointments === null) {
                return [];
            }
            return JSON.parse(appointments);
        }
        
        // Load and display appointments
        function loadAppointments() {
            const appointments = getAppointments();
            
            if (appointments.length === 0) {
                appointmentList.innerHTML = `
                    <div class="empty-state">
                        <i class="far fa-calendar-times"></i>
                        <h3>No Appointments Yet</h3>
                        <p>Book your first appointment using the form on the left.</p>
                    </div>
                `;
                return;
            }
            
            // Sort appointments by date (soonest first)
            appointments.sort((a, b) => new Date(a.appointmentDate) - new Date(b.appointmentDate));
            
            // Generate HTML for appointments
            let html = '';
            appointments.forEach(appointment => {
                // Format date for display
                const date = new Date(appointment.appointmentDate);
                const formattedDate = date.toLocaleDateString('en-US', { 
                    weekday: 'short', 
                    year: 'numeric', 
                    month: 'short', 
                    day: 'numeric' 
                });
                
                html += `
                    <div class="appointment-item">
                        <div class="appointment-header">
                            <span class="appointment-doctor">${appointment.doctor}</span>
                            <span class="appointment-date">${formattedDate} at ${appointment.appointmentTime}</span>
                        </div>
                        <div class="appointment-patient">
                            <strong>Patient:</strong> ${appointment.patientName}
                        </div>
                        <div class="appointment-reason">
                            <strong>Reason:</strong> ${appointment.reason || 'Not specified'}
                        </div>
                        <div class="appointment-actions">
                            <button class="btn-small" onclick="editAppointment(${appointment.id})">
                                <i class="fas fa-edit"></i> Edit
                            </button>
                            <button class="btn-small btn-danger" onclick="deleteAppointment(${appointment.id})">
                                <i class="fas fa-trash"></i> Cancel
                            </button>
                        </div>
                    </div>
                `;
            });
            
            appointmentList.innerHTML = html;
        }
        
        // Delete an appointment
        function deleteAppointment(id) {
            if (confirm('Are you sure you want to cancel this appointment?')) {
                let appointments = getAppointments();
                appointments = appointments.filter(appointment => appointment.id !== id);
                localStorage.setItem('appointments', JSON.stringify(appointments));
                loadAppointments();
                showNotification('Appointment cancelled successfully!', 'success');
            }
        }
        
        // Edit an appointment (placeholder function)
        function editAppointment(id) {
            alert('Edit functionality would be implemented here for appointment ID: ' + id);
            // In a real application, this would populate the form with the appointment data
            // and change the submit button to "Update Appointment"
        }
        
        // Show notification
        function showNotification(message, type = 'success') {
            notification.textContent = message;
            notification.className = 'notification';
            
            if (type === 'error') {
                notification.classList.add('error');
            }
            
            notification.classList.add('show');
            
            setTimeout(() => {
                notification.classList.remove('show');
            }, 3000);
        }
    </script>
</body>
</html>
