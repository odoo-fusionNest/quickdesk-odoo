<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QuickDesk Pro</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        :root {
            --primary: #2b98e0;
            --secondary: #343a40;
            --light-bg: #f8f9fa;
            --card-bg: #ffffff;
        }
        
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background-color: var(--light-bg);
            color: #333;
        }
        
        .header {
            background-color: var(--secondary);
            color: white;
            padding: 1rem;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .logo {
            font-size: 1.5rem;
            font-weight: bold;
            color: white;
        }
        
        .btn-primary {
            background-color: var(--primary);
            border-color: var(--primary);
        }
        
        .container {
            padding: 2rem;
        }
        
        /* Ticket status indicators */
        .status-open {
            border-left: 4px solid #2196F3;
            background-color: #e3f2fd;
        }
        
        .status-in-progress {
            border-left: 4px solid #FFC107;
            background-color: #fff8e1;
        }
        
        .status-resolved {
            border-left: 4px solid #4CAF50;
            background-color: #e8f5e9;
        }
        
        .ticket-card {
            margin-bottom: 1rem;
            padding: 1rem;
            background-color: var(--card-bg);
            border-radius: 8px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
            transition: transform 0.2s;
        }
        
        .ticket-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        
        /* Dashboard sections */
        .dashboard-section {
            display: none;
        }
        
        .active-section {
            display: block;
        }
        
        /* Form elements */
        input, select, textarea {
            margin-bottom: 1rem;
            padding: 0.5rem;
            border: 1px solid #ddd;
            border-radius: 4px;
            width: 100%;
        }
        
        /* Utility classes */
        .hidden {
            display: none;
        }
        
        .text-muted {
            color: #6c757d;
        }
    </style>
</head>
<body>
    <!-- Header with logo and auth button -->
    <header class="header">
        <div class="container-fluid d-flex justify-content-between align-items-center">
            <div class="logo">QuickDesk Pro</div>
            <button class="btn btn-primary" id="authButton">Login / Register</button>
        </div>
    </header>

    <div class="container">
        <!-- Main Dashboard Content -->
        <div id="mainDashboard" class="dashboard-section active-section">
            <h2 class="mb-4">Help Desk Dashboard</h2>
            
            <div class="row">
                <!-- Tickets List Column -->
                <div class="col-md-8">
                    <div class="d-flex justify-content-between align-items-center mb-3">
                        <h4>Your Tickets</h4>
                        <input type="text" id="ticketSearch" class="form-control w-auto" placeholder="Search tickets...">
                    </div>
                    <div id="ticketsList">
                        <!-- Tickets will be loaded here dynamically -->
                    </div>
                </div>
                
                <!-- New Ticket Form Column -->
                <div class="col-md-4">
                    <div class="card">
                        <div class="card-body">
                            <h4>Create New Ticket</h4>
                            <form id="newTicketForm">
                                <input type="text" id="ticketSubject" placeholder="Subject" required>
                                <textarea id="ticketDescription" rows="3" placeholder="Description" required></textarea>
                                <select id="ticketCategory" required>
                                    <option value="">Select Category</option>
                                    <option value="Technical">Technical</option>
                                    <option value="Billing">Billing</option>
                                    <option value="General">General Inquiry</option>
                                </select>
                                <select id="ticketPriority" required>
                                    <option value="Low">Low</option> 
                                    <option value="Medium" selected>Medium</option>
                                    <option value="High">High</option>
                                    <option value="Critical">Critical</option>
                                </select>
                                <button type="submit" class="btn btn-primary">Submit Ticket</button>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Admin Dashboard (hidden by default) -->
        <div id="adminDashboard" class="dashboard-section">
            <h2>Admin Dashboard</h2>
            <div class="card mb-3">
                <div class="card-body">
                    <h4>System Analytics</h4>
                    <div class="d-flex justify-content-around">
                        <div class="text-center">
                            <h5 id="totalTickets">0</h5>
                            <small>Total Tickets</small>
                        </div>
                        <div class="text-center">
                            <h5 id="openTickets">0</h5>
                            <small>Open</small>
                        </div>
                        <div class="text-center">
                            <h5 id="resolvedTickets">0</h5>
                            <small>Resolved</small>
                        </div>
                    </div>
                </div>
            </div>
            <div class="card">
                <div class="card-body">
                    <h4>User Management</h4>
                    <button class="btn btn-outline-primary mb-3">View All Users</button>
                    <button class="btn btn-outline-primary mb-3">Create New User</button>
                    <button class="btn btn-outline-primary mb-3">System Settings</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Auth Modal -->
    <div class="modal fade" id="authModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">QuickDesk Authentication</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <ul class="nav nav-tabs" id="authTabs">
                        <li class="nav-item">
                            <a class="nav-link active" data-bs-toggle="tab" href="#loginTab">Login</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link" data-bs-toggle="tab" href="#registerTab">Register</a>
                        </li>
                    </ul>
                    
                    <div class="tab-content py-3">
                        <!-- Login Form -->
                        <div class="tab-pane active" id="loginTab">
                            <form id="loginForm">
                                <select class="form-select mb-3" id="loginRole">
                                    <option value="user">End User</option>
                                    <option value="agent">Support Agent</option>
                                    <option value="admin">Admin</option>
                                </select>
                                <input type="text" id="loginUsername" placeholder="Username" required>
                                <input type="password" id="loginPassword" placeholder="Password" required>
                                <button type="submit" class="btn btn-primary">Login</button>
                            </form>
                        </div>
                        
                        <!-- Registration Form -->
                        <div class="tab-pane" id="registerTab">
                            <form id="registerForm">
                                <input type="text" id="registerName" placeholder="Full Name" required>
                                <input type="email" id="registerEmail" placeholder="Email" required>
                                <input type="text" id="registerUsername" placeholder="Username" required>
                                <input type="password" id="registerPassword" placeholder="Password" required>
                                <select class="form-select mb-3" id="registerRole">
                                    <option value="user">End User</option>
                                    <option value="agent">Support Agent</option>
                                </select>
                                <button type="submit" class="btn btn-primary">Register</button>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Bootstrap JS Bundle with Popper -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    
    <!-- Main Application Script -->
    <script>
        // Current user state
        let currentUser = null;
        
        // DOM Ready
        document.addEventListener('DOMContentLoaded', function() {
            // Initialize auth modal
            const authModal = new bootstrap.Modal(document.getElementById('authModal'));
            
            // Auth button click handler
            document.getElementById('authButton').addEventListener('click', function() {
                authModal.show();
            });
            
            // Ticket form submission
            document.getElementById('newTicketForm').addEventListener('submit', function(e) {
                e.preventDefault();
                raiseTicket();
            });
            
            // Login form submission
            document.getElementById('loginForm').addEventListener('submit', function(e) {
                e.preventDefault();
                login();
                authModal.hide();
            });
            
            // Register form submission
            document.getElementById('registerForm').addEventListener('submit', function(e) {
                e.preventDefault();
                register();
                authModal.hide();
            });
            
            // Load any existing tickets
            loadTickets();
        });
        
        // Login function
        function login() {
            const username = document.getElementById('loginUsername').value;
            const role = document.getElementById('loginRole').value;
            
            if (!username) {
                alert('Please enter a username');
                return;
            }
            
            currentUser = {
                username: username,
                role: role
            };
            
            updateUIForUser();
        }
        
        // Register function
        function register() {
            const name = document.getElementById('registerName').value;
            const email = document.getElementById('registerEmail').value;
            const username = document.getElementById('registerUsername').value;
            const password = document.getElementById('registerPassword').value;
            const role = document.getElementById('registerRole').value;
            
            if (!name || !email || !username || !password) {
                alert('Please fill in all fields');
                return;
            }
            
            // In a real app, you would send this to a server
            currentUser = {
                name: name,
                email: email,
                username: username,
                role: role
            };
            
            alert(`Registration successful! Welcome ${name}`);
            updateUIForUser();
        }
        
        // Update UI based on current user
        function updateUIForUser() {
            if (!currentUser) return;
            
            // Update auth button
            document.getElementById('authButton').textContent = `Hi, ${currentUser.username} (${currentUser.role})`;
            
            // Show appropriate dashboard
            document.getElementById('mainDashboard').classList.add('active-section');
            
            if (currentUser.role === 'admin') {
                document.getElementById('adminDashboard').classList.add('active-section');
            } else {
                document.getElementById('adminDashboard').classList.remove('active-section');
            }
            
            // Refresh tickets
            loadTickets();
        }
        
        // Raise new ticket
        function raiseTicket() {
            const subject = document.getElementById('ticketSubject').value;
            const description = document.getElementById('ticketDescription').value;
            const category = document.getElementById('ticketCategory').value;
            
            if (!subject || !description || !category) {
                alert('Please fill in all ticket fields');
                return;
            }
            
            if (!currentUser) {
                alert('Please login first');
                return;
            }
            
            // Get existing tickets or initialize empty array
            const tickets = JSON.parse(localStorage.getItem('tickets')) || [];
            
            // Add new ticket
            tickets.push({
                id: Date.now(),
                subject: subject,
                description: description,
                category: category,
                status: 'Open',
                createdBy: currentUser.username,
                createdAt: new Date().toISOString(),
                comments: []
            });
            
            // Save back to localStorage
            localStorage.setItem('tickets', JSON.stringify(tickets));
            
            // Clear form
            document.getElementById('newTicketForm').reset();
            
            // Refresh ticket list
            loadTickets();
            
            alert('Ticket submitted successfully!');
        }
        
        // Load tickets into UI
        function loadTickets() {
            const ticketsList = document.getElementById('ticketsList');
            ticketsList.innerHTML = '';
            
            // Get tickets from localStorage
            const tickets = JSON.parse(localStorage.getItem('tickets')) || [];
            
            if (tickets.length === 0) {
                ticketsList.innerHTML = '<p>No tickets found</p>';
                return;
            }
            
            // Filter tickets based on user role
            let filteredTickets = tickets;
            if (currentUser && currentUser.role !== 'admin') {
                filteredTickets = tickets.filter(ticket => 
                    ticket.createdBy === currentUser.username
                );
            }
            
            // Create ticket cards
            filteredTickets.forEach(ticket => {
                const ticketCard = document.createElement('div');
                ticketCard.className = `ticket-card status-${ticket.status.toLowerCase().replace(' ', '-')}`;
                
                ticketCard.innerHTML = `
                    <h5>${ticket.subject}</h5>
                    <p>${ticket.description}</p>
                    <div class="d-flex justify-content-between mt-2">
                        <small class="text-muted">Category: ${ticket.category}</small>
                        <small class="text-muted">Status: ${ticket.status}</small>
                    </div>
                    <div class="d-flex justify-content-between mt-2">
                        <small class="text-muted">Created: ${new Date(ticket.createdAt).toLocaleDateString()}</small>
                        <small class="text-muted">By: ${ticket.createdBy}</small>
                    </div>
                    ${currentUser && currentUser.role === 'agent' ? `
                    <div class="mt-3">
                        <button class="btn btn-sm btn-warning me-2" onclick="updateTicketStatus(${ticket.id}, 'In Progress')">Mark In Progress</button>
                        <button class="btn btn-sm btn-success" onclick="updateTicketStatus(${ticket.id}, 'Resolved')">Resolve</button>
                    </div>
                    ` : ''}
                `;
                
                ticketsList.appendChild(ticketCard);
            });
        }
        
        // Update ticket status
        function updateTicketStatus(ticketId, newStatus) {
            const tickets = JSON.parse(localStorage.getItem('tickets')) || [];
            const ticketIndex = tickets.findIndex(ticket => ticket.id === ticketId);
            
            if (ticketIndex !== -1) {
                tickets[ticketIndex].status = newStatus;
                localStorage.setItem('tickets', JSON.stringify(tickets));
                loadTickets();
            }
        }
    </script>
</body>
</html>
