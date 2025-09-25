# Timebanditsinside.nl
Interne informatie voor TB band
<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Time Bandits Inside</title>
    <link href="https://fonts.googleapis.com/css2?family=Audiowide&family=Inter:wght@400;700&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            color: white;
            background-image: linear-gradient(135deg, rgba(76, 29, 149, 1) 0%, rgba(29, 78, 216, 1) 100%);
        }
        
        .menu-link {
            transition: all 0.3s ease;
            position: relative;
        }

        .menu-link:hover {
            color: #ff00ff;
            transform: scale(1.1);
        }

        .menu-link:hover::before {
            content: '*';
            position: absolute;
            left: -1rem;
            top: 50%;
            transform: translateY(-50%);
            font-size: 1.5rem;
            color: #ff00ff;
            animation: glitter 0.5s infinite;
        }
        
        .menu-link:hover::after {
            content: '*';
            position: absolute;
            right: -1rem;
            top: 50%;
            transform: translateY(-50%);
            font-size: 1.5rem;
            color: #ff00ff;
            animation: glitter 0.5s infinite reverse;
        }
        
        .menu-link.active {
            color: #ff00ff;
        }

        @keyframes glitter {
            0% { opacity: 0; }
            50% { opacity: 1; }
            100% { opacity: 0; }
        }

        .star-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .star {
            position: absolute;
            background-color: white;
            border-radius: 50%;
            animation: star-fall linear infinite;
        }

        @keyframes star-fall {
            0% {
                transform: translateY(-100vh) rotate(0deg);
            }
            100% {
                transform: translateY(100vh) rotate(360deg);
            }
        }

        .content-section {
            display: none;
        }

        .content-section.active {
            display: block;
        }
        
        /* Form styling */
        .form-input, .form-select {
            background-color: rgba(255, 255, 255, 0.1);
            border: 2px solid rgba(255, 255, 255, 0.3);
            color: white;
            padding: 0.5rem;
            border-radius: 0.5rem;
            outline: none;
            transition: all 0.3s ease;
        }

        .form-input:focus, .form-select:focus {
            border-color: #ff00ff;
            box-shadow: 0 0 5px #ff00ff;
        }

        .form-button {
            background-color: #ff00ff;
            color: black;
            font-weight: bold;
            padding: 0.75rem 1.5rem;
            border-radius: 0.5rem;
            transition: all 0.3s ease;
        }

        .form-button:hover {
            transform: scale(1.05);
            background-color: #e600e6;
        }

        .table-header {
            background-color: rgba(255, 255, 255, 0.2);
        }

        .table-row {
            background-color: rgba(255, 255, 255, 0.1);
        }

        #user-name, #event-type, #filter-month, #filter-type, #filter-name {
            background-color: #add8e6; /* Lichtblauw */
            color: black;
        }

        #user-name option, #event-type option, #filter-month option, #filter-type option, #filter-name option {
            color: black;
            background-color: white;
        }

        .file-upload-section {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .file-upload-input {
            display: none; /* Hide the file input */
        }

        .file-list {
            list-style: none;
            padding: 0;
            width: 100%;
        }

        .file-list li {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 0.75rem;
            border-radius: 0.5rem;
            margin-bottom: 0.5rem;
        }
        
    </style>
</head>
<body class="flex flex-col items-center min-h-screen p-4 overflow-hidden">
    
    <!-- Glitter stars -->
    <div class="star-container"></div>

    <!-- Login Screen -->
    <div id="login-screen" class="z-20 fixed inset-0 flex flex-col items-center justify-center p-4 bg-gray-900/90 backdrop-blur-sm">
        <h2 class="text-3xl sm:text-4xl font-bold mb-6 text-center" style="font-family: 'Audiowide', sans-serif;">Voer de beveiligingscode in</h2>
        <form id="login-form" class="flex flex-col items-center space-y-4">
            <input type="password" id="security-code" placeholder="Beveiligingscode" class="form-input text-center w-full max-w-xs" required>
            <button type="submit" class="form-button w-full max-w-xs">Verzenden</button>
        </form>
        <div id="login-message" class="mt-4 text-red-400"></div>
    </div>
    
    <div id="main-content" class="z-10 flex-col items-center text-center pt-16 hidden">
        <h1 class="text-4xl sm:text-5xl md:text-6xl font-black whitespace-nowrap" style="font-family: 'Audiowide', sans-serif;">
            TIME BANDITS INSIDE
        </h1>
        
        <!-- Navigation Menu -->
        <nav class="mt-6 w-full max-w-lg">
            <ul id="menu" class="flex flex-col sm:flex-row sm:justify-center space-y-4 sm:space-y-0 sm:space-x-8 text-xl sm:text-2xl md:text-3xl font-bold">
                <li><a href="#agenda" class="menu-link block p-3 rounded-lg active" data-content="agenda-content">Agenda</a></li>
                <li><a href="#setlijsten" class="menu-link block p-3 rounded-lg" data-content="setlijsten-content">Setlijsten</a></li>
                <li><a href="#repertoire" class="menu-link block p-3 rounded-lg" data-content="repertoire-content">Repertoire</a></li>
                <li><a href="#bladmuziek" class="menu-link block p-3 rounded-lg" data-content="bladmuziek-content">Bladmuziek</a></li>
                <li><a href="#gebruikers" class="menu-link block p-3 rounded-lg" data-content="gebruikers-content">Gebruikers</a></li>
            </ul>
        </nav>

        <!-- Content Sections -->
        <div class="mt-12 w-full max-w-3xl p-6 rounded-lg bg-gray-900/50 shadow-lg content-section active" id="agenda-content">
            <h2 class="text-3xl font-bold mb-6 text-center">Agenda</h2>
            
            <!-- Agenda Add Form -->
            <form id="agenda-form" class="space-y-4">
                <div class="flex flex-col md:flex-row md:space-x-4 space-y-4 md:space-y-0">
                    <input type="text" id="event-name" placeholder="Naam evenement" class="form-input w-full" required>
                    <input type="date" id="event-date" class="form-input w-full" required>
                    <select id="event-type" class="form-select w-full" required>
                        <option value="optreden">Optreden</option>
                        <option value="optie">Optie</option>
                        <option value="repetitie">Repetitie</option>
                        <option value="bezet">Bezet</option>
                    </select>
                    <select id="user-name" class="form-select w-full" required>
                        <option value="Time Bandits">Time Bandits</option>
                        <option value="Alides">Alides</option>
                        <option value="Sietse">Sietse</option>
                        <option value="Ronald">Ronald</option>
                        <option value="Felix">Felix</option>
                        <option value="Pascal">Pascal</option>
                        <option value="Daniëlle">Daniëlle</option>
                        <option value="Kelly">Kelly</option>
                    </select>
                </div>
                <div class="flex justify-center mt-4">
                    <button type="submit" class="form-button">Voeg evenement toe</button>
                </div>
            </form>

            <div id="message-box" class="mt-4 p-4 text-center rounded-lg hidden"></div>

            <!-- Agenda Filters -->
            <div class="mt-8">
                <h3 class="text-2xl font-bold mb-4">Filter evenementen</h3>
                <div class="flex flex-col md:flex-row md:space-x-4 space-y-4 md:space-y-0 items-center justify-center">
                    <select id="filter-month" class="form-select w-full md:w-auto">
                        <option value="">Maand</option>
                        <option value="1">Januari</option>
                        <option value="2">Februari</option>
                        <option value="3">Maart</option>
                        <option value="4">April</option>
                        <option value="5">Mei</option>
                        <option value="6">Juni</option>
                        <option value="7">Juli</option>
                        <option value="8">Augustus</option>
                        <option value="9">September</option>
                        <option value="10">Oktober</option>
                        <option value="11">November</option>
                        <option value="12">December</option>
                    </select>
                    <select id="filter-type" class="form-select w-full md:w-auto">
                        <option value="">Type</option>
                        <option value="optreden">Optreden</option>
                        <option value="optie">Optie</option>
                        <option value="repetitie">Repetitie</option>
                        <option value="bezet">Bezet</option>
                    </select>
                    <select id="filter-name" class="form-select w-full md:w-auto">
                        <option value="">Naam</option>
                        <option value="Time Bandits">Time Bandits</option>
                        <option value="Alides">Alides</option>
                        <option value="Sietse">Sietse</option>
                        <option value="Ronald">Ronald</option>
                        <option value="Felix">Felix</option>
                        <option value="Pascal">Pascal</option>
                        <option value="Daniëlle">Daniëlle</option>
                        <option value="Kelly">Kelly</option>
                    </select>
                    <button id="filter-button" class="form-button mt-4 md:mt-0">Filter</button>
                    <button id="reset-button" class="form-button mt-4 md:mt-0">Reset</button>
                </div>
            </div>

            <!-- Agenda List -->
            <div class="mt-8">
                <h3 class="text-2xl font-bold mb-4">Evenementenoverzicht</h3>
                <div class="overflow-x-auto rounded-lg">
                    <table class="min-w-full table-auto text-left rounded-lg">
                        <thead class="table-header">
                            <tr>
                                <th class="p-3">Naam evenement</th>
                                <th class="p-3">Datum</th>
                                <th class="p-3">Type</th>
                                <th class="p-3">Naam</th>
                            </tr>
                        </thead>
                        <tbody id="agenda-list">
                            <!-- Events will be added here dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <div class="mt-12 w-full max-w-3xl p-6 rounded-lg bg-gray-900/50 shadow-lg content-section" id="setlijsten-content">
            <h2 class="text-3xl font-bold text-center mb-6">Setlijsten</h2>
            <div class="file-upload-section">
                <label for="setlijsten-file-input" class="form-button cursor-pointer">
                    Upload bestand
                </label>
                <input type="file" id="setlijsten-file-input" class="file-upload-input" accept=".pdf,.doc,.docx,.xls,.xlsx">
                <ul id="setlijsten-file-list" class="file-list mt-6"></ul>
            </div>
        </div>

        <div class="mt-12 w-full max-w-3xl p-6 rounded-lg bg-gray-900/50 shadow-lg content-section" id="repertoire-content">
            <h2 class="text-3xl font-bold text-center mb-6">Repertoire</h2>
            <div class="file-upload-section">
                <label for="repertoire-file-input" class="form-button cursor-pointer">
                    Upload bestand
                </label>
                <input type="file" id="repertoire-file-input" class="file-upload-input" accept=".pdf,.doc,.docx,.xls,.xlsx">
                <ul id="repertoire-file-list" class="file-list mt-6"></ul>
            </div>
        </div>

        <div class="mt-12 w-full max-w-3xl p-6 rounded-lg bg-gray-900/50 shadow-lg content-section" id="bladmuziek-content">
            <h2 class="text-3xl font-bold text-center mb-6">Bladmuziek</h2>
            <div class="file-upload-section">
                <label for="bladmuziek-file-input" class="form-button cursor-pointer">
                    Upload bestand
                </label>
                <input type="file" id="bladmuziek-file-input" class="file-upload-input" accept=".pdf,.doc,.docx,.xls,.xlsx">
                <ul id="bladmuziek-file-list" class="file-list mt-6"></ul>
            </div>
        </div>

        <div class="mt-12 w-full max-w-3xl p-6 rounded-lg bg-gray-900/50 shadow-lg content-section" id="gebruikers-content">
            <h2 class="text-3xl font-bold mb-6 text-center">Gebruikers</h2>
            
            <!-- Gebruikers Add Form -->
            <form id="users-form" class="flex flex-col md:flex-row md:space-x-4 space-y-4 md:space-y-0 items-center justify-center">
                <input type="text" id="new-user-name" placeholder="Voer naam in" class="form-input w-full" required>
                <button type="submit" class="form-button w-full md:w-auto">Voeg gebruiker toe</button>
            </form>

            <!-- Gebruikers List -->
            <div class="mt-8">
                <h3 class="text-2xl font-bold mb-4">Lijst van gebruikers</h3>
                <ul id="users-list" class="list-disc list-inside space-y-2 text-left"></ul>
            </div>
        </div>
    </div>
    
    <script>
        // JavaScript for dynamic star glitter effect and content switching
        function createStars() {
            const container = document.querySelector('.star-container');
            const numStars = 50;
            for (let i = 0; i < numStars; i++) {
                const star = document.createElement('div');
                star.className = 'star';
                const size = Math.random() * 3 + 1;
                star.style.width = `${size}px`;
                star.style.height = `${size}px`;
                star.style.top = `${Math.random() * 100}%`;
                star.style.left = `${Math.random() * 100}%`;
                star.style.animationDuration = `${Math.random() * 10 + 5}s`;
                star.style.animationDelay = `-${Math.random() * 5}s`;
                container.appendChild(star);
            }
        }
        
        document.addEventListener('DOMContentLoaded', () => {
            createStars();

            const loginScreen = document.getElementById('login-screen');
            const mainContent = document.getElementById('main-content');
            const loginForm = document.getElementById('login-form');
            const securityCodeInput = document.getElementById('security-code');
            const loginMessage = document.getElementById('login-message');

            const menuLinks = document.querySelectorAll('#menu a');
            const contentSections = document.querySelectorAll('.content-section');
            const agendaForm = document.getElementById('agenda-form');
            const agendaList = document.getElementById('agenda-list');
            const messageBox = document.getElementById('message-box');
            const usersForm = document.getElementById('users-form');
            const usersList = document.getElementById('users-list');
            const userNameSelect = document.getElementById('user-name');
            const filterNameSelect = document.getElementById('filter-name');
            const filterMonthSelect = document.getElementById('filter-month');
            const filterTypeSelect = document.getElementById('filter-type');
            const filterButton = document.getElementById('filter-button');
            const resetButton = document.getElementById('reset-button');

            // File upload elements
            const setlijstenFileInput = document.getElementById('setlijsten-file-input');
            const setlijstenFileList = document.getElementById('setlijsten-file-list');
            const repertoireFileInput = document.getElementById('repertoire-file-input');
            const repertoireFileList = document.getElementById('repertoire-file-list');
            const bladmuziekFileInput = document.getElementById('bladmuziek-file-input');
            const bladmuziekFileList = document.getElementById('bladmuziek-file-list');
            
            let events = [];
            const initialUsers = ["Time Bandits", "Alides", "Sietse", "Ronald", "Felix", "Pascal", "Daniëlle", "Kelly"];
            let users = [...initialUsers];

            function renderUsers() {
                usersList.innerHTML = '';
                userNameSelect.innerHTML = '';
                filterNameSelect.innerHTML = '<option value="">Naam</option>';

                users.forEach(user => {
                    const listItem = document.createElement('li');
                    listItem.textContent = user;
                    usersList.appendChild(listItem);
                    
                    const option = document.createElement('option');
                    option.value = user;
                    option.textContent = user;
                    userNameSelect.appendChild(option);

                    const filterOption = option.cloneNode(true);
                    filterNameSelect.appendChild(filterOption);
                });
            }

            renderUsers();
            renderEvents(events);

            loginForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const code = securityCodeInput.value.trim();
                const correctCode = 'Alides54';

                if (code === correctCode) {
                    loginScreen.style.display = 'none';
                    mainContent.style.display = 'flex';
                } else {
                    loginMessage.textContent = 'Verkeerde code. Probeer het opnieuw.';
                }
            });

            menuLinks.forEach(link => {
                link.addEventListener('click', (e) => {
                    e.preventDefault();
                    
                    // Verwijder 'active' class van alle links
                    menuLinks.forEach(item => item.classList.remove('active'));
                    
                    // Voeg 'active' class toe aan de geklikte link
                    e.target.classList.add('active');
                    
                    // Verberg alle contentsecties
                    contentSections.forEach(section => section.classList.remove('active'));
                    
                    // Toon de bijbehorende contentsectie
                    const contentId = e.target.getAttribute('data-content');
                    const targetSection = document.getElementById(contentId);
                    if (targetSection) {
                        targetSection.classList.add('active');
                    }
                });
            });

            usersForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const newUser = document.getElementById('new-user-name').value.trim();
                if (newUser && !users.includes(newUser)) {
                    users.push(newUser);
                    renderUsers();
                    document.getElementById('new-user-name').value = '';
                }
            });

            agendaForm.addEventListener('submit', (e) => {
                e.preventDefault();

                const eventName = document.getElementById('event-name').value;
                const eventDate = document.getElementById('event-date').value;
                const eventType = document.getElementById('event-type').value;
                const userName = document.getElementById('user-name').value;

                if (!eventName || !eventDate || !eventType || !userName) {
                    showMessage("Alle velden moeten worden ingevuld.", "bg-red-500");
                    return;
                }

                const newEvent = {
                    name: eventName,
                    date: eventDate,
                    type: eventType,
                    user: userName
                };
                events.push(newEvent);
                renderEvents(events);
                agendaForm.reset();
                showMessage("Evenement succesvol toegevoegd!", "bg-green-500");
            });

            function renderEvents(eventsToRender) {
                agendaList.innerHTML = '';
                eventsToRender.forEach(event => {
                    const newRow = document.createElement('tr');
                    newRow.className = 'table-row';
                    newRow.innerHTML = `
                        <td class="p-3 border-t border-gray-700">${event.name}</td>
                        <td class="p-3 border-t border-gray-700">${event.date}</td>
                        <td class="p-3 border-t border-gray-700">${event.type}</td>
                        <td class="p-3 border-t border-gray-700">${event.user}</td>
                    `;
                    agendaList.appendChild(newRow);
                });
            }

            function showMessage(message, colorClass) {
                messageBox.textContent = message;
                messageBox.className = `mt-4 p-4 text-center rounded-lg ${colorClass}`;
                messageBox.style.display = 'block';
                setTimeout(() => {
                    messageBox.style.display = 'none';
                }, 3000); // Verberg na 3 seconden
            }

            filterButton.addEventListener('click', () => {
                const selectedMonth = filterMonthSelect.value;
                const selectedType = filterTypeSelect.value;
                const selectedName = filterNameSelect.value;

                const filteredEvents = events.filter(event => {
                    const eventMonth = new Date(event.date).getMonth() + 1;
                    const monthMatch = !selectedMonth || parseInt(selectedMonth) === eventMonth;
                    const typeMatch = !selectedType || selectedType === event.type;
                    const nameMatch = !selectedName || selectedName === event.user;
                    
                    return monthMatch && typeMatch && nameMatch;
                });
                
                renderEvents(filteredEvents);
            });

            resetButton.addEventListener('click', () => {
                filterMonthSelect.value = '';
                filterTypeSelect.value = '';
                filterNameSelect.value = '';
                renderEvents(events);
            });

            // Handle file uploads
            function handleFileUpload(input, fileListElement) {
                input.addEventListener('change', (e) => {
                    const file = e.target.files[0];
                    if (file) {
                        const fileExtension = file.name.split('.').pop().toLowerCase();
                        let fileTypeIcon = '';
                        if (['pdf'].includes(fileExtension)) {
                            fileTypeIcon = '📄'; // PDF icon
                        } else if (['doc', 'docx'].includes(fileExtension)) {
                            fileTypeIcon = '📝'; // Word icon
                        } else if (['xls', 'xlsx'].includes(fileExtension)) {
                            fileTypeIcon = '📊'; // Excel icon
                        }

                        const listItem = document.createElement('li');
                        listItem.textContent = `${fileTypeIcon} ${file.name}`;
                        fileListElement.appendChild(listItem);
                    }
                });
            }
            
            handleFileUpload(setlijstenFileInput, setlijstenFileList);
            handleFileUpload(repertoireFileInput, repertoireFileList);
            handleFileUpload(bladmuziekFileInput, bladmuziekFileList);
        });
    </script>
</body>
</html>
