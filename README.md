<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Digital Life Watch</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Base styles for the body to center the watch and set background */
        body {
            font-family: "Inter", sans-serif; /* Preferred font */
            background-color: #1a202c; /* Dark background for a watch face aesthetic */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh; /* Full viewport height */
            padding: 10px; /* Reduced padding for smaller screens */
            color: #e2e8f0; /* Light text color for contrast */
            overflow-y: auto; /* Allow scrolling if content is too tall */
        }
        /* Container for the watch face elements */
        .watch-container {
            background-color: #2d3748; /* Slightly lighter dark background for the watch itself */
            padding: 20px; /* Adjusted padding for mobile */
            border-radius: 20px; /* Rounded corners for a sleek look */
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.4); /* Deep shadow for depth */
            text-align: left; /* Ensures text inside is left-aligned */
            max-width: 500px; /* Increased max width to accommodate inputs */
            width: 100%; /* Responsive width */
            border: 2px solid #4a5568; /* Subtle border */
            display: flex;
            flex-direction: column;
            gap: 15px; /* Adjusted spacing between countdown items */
            justify-content: center;
            align-items: flex-start; /* Aligns items (like input-groups, countdown-items) to the start (left) */
        }
        /* Styling for input groups */
        .input-group {
            width: 100%;
            text-align: left;
            margin-bottom: 10px; /* Adjusted margin */
        }
        .input-group label {
            display: block;
            font-size: 0.85rem; /* Slightly smaller label font on mobile */
            font-weight: 500;
            color: #cbd5e0;
            margin-bottom: 5px;
        }
        .input-group input[type="number"] {
            width: 100%;
            padding: 8px 12px; /* Adjusted padding for inputs */
            border-radius: 8px;
            border: 1px solid #4a5568;
            background-color: #4a5568;
            color: #e2e8f0;
            font-size: 0.95rem; /* Adjusted font size for inputs */
            -moz-appearance: textfield; /* Firefox */
        }
        .input-group input::-webkit-outer-spin-button,
        .input-group input::-webkit-inner-spin-button {
            -webkit-appearance: none; /* Chrome, Safari, Edge */
            margin: 0;
        }
        .input-group input[type="number"]:focus {
            outline: none;
            border-color: #63b3ed;
            box-shadow: 0 0 0 2px rgba(99, 179, 237, 0.5);
        }
        .input-flex-group {
            display: flex;
            flex-wrap: wrap; /* Allow wrapping on very small screens */
            gap: 8px; /* Adjusted gap */
            width: 100%;
        }
        .input-flex-group > div {
            flex: 1;
            min-width: 80px; /* Ensure inputs don't become too narrow */
        }
        .save-button {
            background-color: #4299e1; /* Blue-500 */
            color: white;
            padding: 10px 20px; /* Adjusted padding */
            border-radius: 10px;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.3s ease, transform 0.2s ease;
            border: none;
            width: 100%;
            margin-top: 10px;
        }
        .save-button:hover {
            background-color: #3182ce; /* Blue-600 */
            transform: translateY(-2px);
        }
        .save-button:active {
            transform: translateY(0);
        }
        .message-box {
            margin-top: 10px; /* Adjusted margin */
            padding: 8px 12px; /* Adjusted padding */
            border-radius: 8px;
            font-size: 0.85rem; /* Adjusted font size */
            font-weight: 500;
            text-align: center;
        }
        .message-box.success {
            background-color: #48bb78; /* Green-500 */
            color: white;
        }
        .message-box.error {
            background-color: #e53e3e; /* Red-600 */
            color: white;
        }

        /* Styling for each individual countdown display item */
        .countdown-item {
            display: flex; /* Changed to flex */
            flex-direction: row; /* Arranges children in a row */
            justify-content: space-between; /* Pushes value to left, label to right */
            align-items: center; /* Vertically centers label and value */
            width: 100%;
            padding: 5px 0; /* Add some vertical padding for spacing */
            border-bottom: 1px dashed rgba(255, 255, 255, 0.1); /* Subtle separator */
        }
        .countdown-item:last-child {
            border-bottom: none; /* No border for the last item */
        }
        /* Labels for the countdown values (e.g., "Sunsets Left") */
        .countdown-label {
            font-size: 0.95rem; /* Adjusted to be closer to input label size */
            font-weight: 500; /* font-medium in Tailwind */
            color: #cbd5e0; /* text-gray-300 in Tailwind */
            margin-bottom: 0; /* No margin-bottom when in a row */
            flex-shrink: 0; /* Prevent label from shrinking */
            padding-left: 10px; /* Space between value and label */
        }
        /* The actual countdown numbers - unified with input font size */
        .countdown-value {
            font-family: 'monospace'; /* Using monospace for reliable digital look */
            font-size: 0.95rem; /* Unified with input font size */
            font-weight: 700; /* font-bold in Tailwind */
            color: #63b3ed; /* text-blue-300 in Tailwind */
            text-shadow: 0 0 8px rgba(99, 179, 237, 0.6); /* Subtle glow effect */
            line-height: 1; /* Compact line height */
            text-align: left; /* Ensure value is left-aligned within its space */
            flex-grow: 1; /* Allow value to take up remaining space */
        }

        /* Styling for the results title */
        .results-title {
            font-size: 1.1rem; /* Slightly larger than labels, smaller than main title */
            font-weight: 600;
            color: #e2e8f0;
            text-align: center; /* Center the title */
            margin-bottom: 15px; /* Space below the title */
            margin-top: 10px; /* Space above the title */
        }


        /* Media queries for larger screens (desktop) */
        @media (min-width: 640px) { /* Tailwind's 'sm' breakpoint */
            body {
                padding: 20px;
            }
            .watch-container {
                padding: 40px;
                gap: 20px;
            }
            .input-group label {
                font-size: 0.9rem;
            }
            .input-group input[type="number"] {
                padding: 10px 15px;
                font-size: 1rem;
            }
            .input-flex-group {
                gap: 10px;
            }
            .save-button {
                padding: 12px 25px;
            }
            .message-box {
                padding: 10px 15px;
                font-size: 0.9rem;
            }
            .countdown-label {
                font-size: 1.0rem; /* Slightly larger on desktop */
            }
            .countdown-value {
                font-size: 1.2rem; /* Larger on desktop */
            }
            .results-title {
                font-size: 1.3rem;
            }
        }
    </style>
</head>
<body class="bg-gray-900 flex items-center justify-center min-h-screen p-4">
    <div class="watch-container">
        <h1 class="text-3xl font-extrabold text-blue-200 mb-4">Life Countdown</h1>

        <!-- Input Fields Section -->
        <div class="input-group">
            <label>Your Birth Date:</label>
            <div class="input-flex-group">
                <div>
                    <input type="number" id="birthMonthInput" placeholder="MM" min="1" max="12">
                </div>
                <div>
                    <input type="number" id="birthDayInput" placeholder="DD" min="1" max="31">
                </div>
                <div>
                    <input type="number" id="birthYearInput" placeholder="YYYY" min="1900" max="2025">
                </div>
            </div>
        </div>
        <div class="input-group">
            <label for="lifespanInput">Estimated Lifespan (Years):</label>
            <input type="number" id="lifespanInput" placeholder="e.g., 85" min="1" max="120">
        </div>

        <hr class="w-full border-t border-gray-600 my-8"> <!-- Separator for Spouse Info -->

        <div class="input-group">
            <label>Spouse's Birth Date:</label>
            <div class="input-flex-group">
                <div>
                    <input type="number" id="spouseBirthMonthInput" placeholder="MM" min="1" max="12">
                </div>
                <div>
                    <input type="number" id="spouseBirthDayInput" placeholder="DD" min="1" max="31">
                </div>
                <div>
                    <input type="number" id="spouseBirthYearInput" placeholder="YYYY" min="1900" max="2025">
                </div>
            </div>
        </div>
        <div class="input-group">
            <label for="spouseLifespanInput">Spouse's Estimated Lifespan (Years):</label>
            <input type="number" id="spouseLifespanInput" placeholder="e.g., 88" min="1" max="120">
        </div>

        <hr class="w-full border-t border-gray-600 my-8"> <!-- Separator for Other Inputs -->

        <div class="input-group">
            <label for="retirementYearInput">Target Retirement Year:</label>
            <input type="number" id="retirementYearInput" placeholder="e.g., 2055" min="1900" max="2100">
        </div>
        <div class="input-group">
            <label for="tvHoursPerDayInput">Hours of TV/Streaming per Day:</label>
            <input type="number" id="tvHoursPerDayInput" placeholder="e.g., 2" min="0" max="24">
        </div>
        <div class="input-group">
            <label for="surfSessionsPerWeekInput">Surf Sessions per Week:</label>
            <input type="number" id="surfSessionsPerWeekInput" placeholder="e.g., 3" min="0" max="7">
        </div>
        <div class="input-group">
            <label for="vacationsPerYearInput">Vacations per Year:</label>
            <input type="number" id="vacationsPerYearInput" placeholder="e.g., 2" min="0" max="52">
        </div>
        <button id="saveSettingsBtn" class="save-button">Save Settings</button>
        <div id="inputMessageBox" class="message-box hidden"></div>

        <hr class="w-full border-t border-gray-600 my-8"> <!-- Separator -->

        <!-- Results Title -->
        <h2 class="results-title">Time is our most valuable asset.</h2>

        <!-- Countdown Display Section -->
        <div class="countdown-item">
            <span id="sunsetsLeft" class="countdown-value">--</span>
            <span class="countdown-label">🌅</span>
        </div>

        <div class="countdown-item">
            <span id="activeHoursLeftWithoutTV" class="countdown-value">--</span>
            <span class="countdown-label">Active Hours (-😴, -💼)</span>
        </div>

        <div class="countdown-item">
            <span id="activeHoursLeftWithTV" class="countdown-value">--</span>
            <span class="countdown-label">Hours (-😴, -💼, -📺)</span>
        </div>

        <div class="countdown-item">
            <span id="birthdaysLeft" class="countdown-value">--</span>
            <span class="countdown-label">🎂</span>
        </div>

        <div class="countdown-item">
            <span id="christmasesLeft" class="countdown-value">--</span>
            <span class="countdown-label">🎄</span>
        </div>

        <div class="countdown-item">
            <span id="newYearsLeft" class="countdown-value">--</span>
            <span class="countdown-label">🎉</span>
        </div>

        <div class="countdown-item">
            <span id="surfSessionsTotal" class="countdown-value">--</span>
            <span class="countdown-label">🏄‍♀️ Stoked</span>
        </div>

        <hr class="w-full border-t border-gray-600 my-8"> <!-- Separator for Together Time -->

        <h2 class="results-title">Time Left Together</h2>

        <div class="countdown-item">
            <span id="togetherDaysLeft" class="countdown-value">--</span>
            <span class="countdown-label">Days Together</span>
        </div>
        <div class="countdown-item">
            <span id="togetherActiveHoursLeft" class="countdown-value">--</span>
            <span class="countdown-label">Hours Together</span>
        </div>
        <div class="countdown-item">
            <span id="morningCoffeeLeft" class="countdown-value">--</span>
            <span class="countdown-label">☕ Morning Coffees Left</span>
        </div>
        <div class="countdown-item">
            <span id="togetherBirthdaysLeft" class="countdown-value">--</span>
            <span class="countdown-label">🎁 Birthdays (Yours & Spouse's)</span>
        </div>
        <div class="countdown-item">
            <span id="togetherValentinesLeft" class="countdown-value">--</span>
            <span class="countdown-label">❤️ Valentine's Days Together</span>
        </div>
        <div class="countdown-item">
            <span id="vacationsRemaining" class="countdown-value">--</span>
            <span class="countdown-label">✈️ Vacations Remaining</span>
        </div>


        <!-- Message display (e.g., "Time's Up!") -->
        <div id="statusMessage" class="message hidden"></div>
    </div>

    <script>
        // --- Configuration (Initial Defaults) ---
        // These will be overridden by saved settings or user input
        let BIRTH_MONTH = 1; // Default to January
        let BIRTH_DAY = 1;   // Default to 1st
        let BIRTH_YEAR = 1990;
        let ESTIMATED_LIFESPAN_YEARS = 85;

        let SPOUSE_BIRTH_MONTH = 1;
        let SPOUSE_BIRTH_DAY = 1;
        let SPOUSE_BIRTH_YEAR = 1990;
        let SPOUSE_LIFESPAN_YEARS = 88;

        let RETIREMENT_YEAR = 2055;
        let TV_HOURS_PER_DAY = 2; // Default hours of TV/streaming per day
        let SURF_SESSIONS_PER_WEEK = 0; // New: Default surf sessions per week
        let VACATIONS_PER_YEAR = 0; // New: Default vacations per year

        const ACTIVE_HOURS_PER_DAY_BASE = 16; // 24 hours - 8 hours for sleep (base active hours before work/TV)
        const WORK_HOURS_PER_WEEK = 40; // Hours worked per week

        // --- Date Variables (will be recalculated) ---
        let estimatedEndDate; // User's estimated end date
        let spouseEstimatedEndDate; // Spouse's estimated end date
        let togetherEndDate; // The earlier of the two end dates

        let retirementDate;
        let userBirthDate;
        let spouseBirthDate;

        // --- DOM Element References ---
        const birthMonthInput = document.getElementById('birthMonthInput');
        const birthDayInput = document.getElementById('birthDayInput');
        const birthYearInput = document.getElementById('birthYearInput');
        const lifespanInput = document.getElementById('lifespanInput');

        const spouseBirthMonthInput = document.getElementById('spouseBirthMonthInput');
        const spouseBirthDayInput = document.getElementById('spouseBirthDayInput');
        const spouseBirthYearInput = document.getElementById('spouseBirthYearInput');
        const spouseLifespanInput = document.getElementById('spouseLifespanInput');

        const retirementYearInput = document.getElementById('retirementYearInput');
        const tvHoursPerDayInput = document.getElementById('tvHoursPerDayInput');
        const surfSessionsPerWeekInput = document.getElementById('surfSessionsPerWeekInput');
        const vacationsPerYearInput = document.getElementById('vacationsPerYearInput'); // New input reference

        const saveSettingsBtn = document.getElementById('saveSettingsBtn');
        const inputMessageBox = document.getElementById('inputMessageBox');

        const sunsetsLeftElement = document.getElementById('sunsetsLeft');
        const activeHoursLeftWithoutTVElement = document.getElementById('activeHoursLeftWithoutTV');
        const activeHoursLeftWithTVElement = document.getElementById('activeHoursLeftWithTV');
        const birthdaysLeftElement = document.getElementById('birthdaysLeft');
        const christmasesLeftElement = document.getElementById('christmasesLeft');
        const newYearsLeftElement = document.getElementById('newYearsLeft');
        const surfSessionsTotalElement = document.getElementById('surfSessionsTotal');

        // New Together Time Elements
        const togetherDaysLeftElement = document.getElementById('togetherDaysLeft');
        const togetherActiveHoursLeftElement = document.getElementById('togetherActiveHoursLeft');
        const morningCoffeeLeftElement = document.getElementById('morningCoffeeLeft');
        const togetherBirthdaysLeftElement = document.getElementById('togetherBirthdaysLeft');
        const togetherValentinesLeftElement = document.getElementById('togetherValentinesLeft'); // New
        const vacationsRemainingElement = document.getElementById('vacationsRemaining'); // New display reference

        const statusMessageElement = document.getElementById('statusMessage');

        /**
         * Displays a message in the input message box.
         * @param {string} message - The message to display.
         * @param {string} type - 'success' or 'error' to apply appropriate styling.
         */
        function showInputMessage(message, type) {
            inputMessageBox.textContent = message;
            inputMessageBox.className = 'message-box'; // Reset classes
            inputMessageBox.classList.add(type);
            inputMessageBox.classList.remove('hidden');
            setTimeout(() => {
                inputMessageBox.classList.add('hidden');
            }, 3000); // Hide after 3 seconds
        }

        /**
         * Loads user settings from localStorage and applies them to inputs and variables.
         */
        function loadSettings() {
            const savedBirthMonth = localStorage.getItem('birthMonth');
            const savedBirthDay = localStorage.getItem('birthDay');
            const savedBirthYear = localStorage.getItem('birthYear');
            const savedLifespan = localStorage.getItem('lifespan');

            const savedSpouseBirthMonth = localStorage.getItem('spouseBirthMonth');
            const savedSpouseBirthDay = localStorage.getItem('spouseBirthDay');
            const savedSpouseBirthYear = localStorage.getItem('spouseBirthYear');
            const savedSpouseLifespan = localStorage.getItem('spouseLifespan');

            const savedRetirementYear = localStorage.getItem('retirementYear');
            const savedTvHoursPerDay = localStorage.getItem('tvHoursPerDay');
            const savedSurfSessionsPerWeek = localStorage.getItem('surfSessionsPerWeek');
            const savedVacationsPerYear = localStorage.getItem('vacationsPerYear'); // Load new setting

            // Load and apply user birth date components
            if (savedBirthMonth) {
                BIRTH_MONTH = parseInt(savedBirthMonth);
                birthMonthInput.value = BIRTH_MONTH;
            } else { birthMonthInput.value = BIRTH_MONTH; }
            if (savedBirthDay) {
                BIRTH_DAY = parseInt(savedBirthDay);
                birthDayInput.value = BIRTH_DAY;
            } else { birthDayInput.value = BIRTH_DAY; }
            if (savedBirthYear) {
                BIRTH_YEAR = parseInt(savedBirthYear);
                birthYearInput.value = BIRTH_YEAR;
            } else { birthYearInput.value = BIRTH_YEAR; }
            if (savedLifespan) {
                ESTIMATED_LIFESPAN_YEARS = parseInt(savedLifespan);
                lifespanInput.value = ESTIMATED_LIFESPAN_YEARS;
            } else { lifespanInput.value = ESTIMATED_LIFESPAN_YEARS; }

            // Load and apply spouse birth date components
            if (savedSpouseBirthMonth) {
                SPOUSE_BIRTH_MONTH = parseInt(savedSpouseBirthMonth);
                spouseBirthMonthInput.value = SPOUSE_BIRTH_MONTH;
            } else { spouseBirthMonthInput.value = SPOUSE_BIRTH_MONTH; }
            if (savedSpouseBirthDay) {
                SPOUSE_BIRTH_DAY = parseInt(savedSpouseBirthDay);
                spouseBirthDayInput.value = SPOUSE_BIRTH_DAY;
            } else { spouseBirthDayInput.value = SPOUSE_BIRTH_DAY; }
            if (savedSpouseBirthYear) {
                SPOUSE_BIRTH_YEAR = parseInt(savedSpouseBirthYear);
                spouseBirthYearInput.value = SPOUSE_BIRTH_YEAR;
            } else { spouseBirthYearInput.value = SPOUSE_BIRTH_YEAR; }
            if (savedSpouseLifespan) {
                SPOUSE_LIFESPAN_YEARS = parseInt(savedSpouseLifespan);
                spouseLifespanInput.value = SPOUSE_LIFESPAN_YEARS;
            } else { spouseLifespanInput.value = SPOUSE_LIFESPAN_YEARS; }


            // Load and apply other settings
            if (savedRetirementYear) {
                RETIREMENT_YEAR = parseInt(savedRetirementYear);
                retirementYearInput.value = RETIREMENT_YEAR;
            } else { retirementYearInput.value = RETIREMENT_YEAR; }
            if (savedTvHoursPerDay) {
                TV_HOURS_PER_DAY = parseFloat(savedTvHoursPerDay);
                tvHoursPerDayInput.value = TV_HOURS_PER_DAY;
            } else { tvHoursPerDayInput.value = TV_HOURS_PER_DAY; }
            if (savedSurfSessionsPerWeek) {
                SURF_SESSIONS_PER_WEEK = parseFloat(savedSurfSessionsPerWeek);
                surfSessionsPerWeekInput.value = SURF_SESSIONS_PER_WEEK;
            } else { surfSessionsPerWeekInput.value = SURF_SESSIONS_PER_WEEK; }
            if (savedVacationsPerYear) { // Load new setting value
                VACATIONS_PER_YEAR = parseFloat(savedVacationsPerYear);
                vacationsPerYearInput.value = VACATIONS_PER_YEAR;
            } else { vacationsPerYearInput.value = VACATIONS_PER_YEAR; }


            // Recalculate dates based on loaded or default values
            recalculateDates();
        }

        /**
         * Saves current settings to localStorage.
         */
        function saveSettings() {
            localStorage.setItem('birthMonth', BIRTH_MONTH);
            localStorage.setItem('birthDay', BIRTH_DAY);
            localStorage.setItem('birthYear', BIRTH_YEAR);
            localStorage.setItem('lifespan', ESTIMATED_LIFESPAN_YEARS);

            localStorage.setItem('spouseBirthMonth', SPOUSE_BIRTH_MONTH);
            localStorage.setItem('spouseBirthDay', SPOUSE_BIRTH_DAY);
            localStorage.setItem('spouseBirthYear', SPOUSE_BIRTH_YEAR);
            localStorage.setItem('spouseLifespan', SPOUSE_LIFESPAN_YEARS);

            localStorage.setItem('retirementYear', RETIREMENT_YEAR);
            localStorage.setItem('tvHoursPerDay', TV_HOURS_PER_DAY);
            localStorage.setItem('surfSessionsPerWeek', SURF_SESSIONS_PER_WEEK);
            localStorage.setItem('vacationsPerYear', VACATIONS_PER_YEAR);
        }

        /**
         * Recalculates all relevant dates based on current global variables.
         */
        function recalculateDates() {
            const currentYear = new Date().getFullYear();

            // User's dates
            userBirthDate = new Date(BIRTH_YEAR, BIRTH_MONTH - 1, BIRTH_DAY);
            estimatedEndDate = new Date(
                userBirthDate.getFullYear() + ESTIMATED_LIFESPAN_YEARS,
                userBirthDate.getMonth(),
                userBirthDate.getDate()
            );

            // Spouse's dates
            spouseBirthDate = new Date(SPOUSE_BIRTH_YEAR, SPOUSE_BIRTH_MONTH - 1, SPOUSE_BIRTH_DAY);
            spouseEstimatedEndDate = new Date(
                spouseBirthDate.getFullYear() + SPOUSE_LIFESPAN_YEARS,
                spouseBirthDate.getMonth(),
                spouseBirthDate.getDate()
            );

            // Time Left Together: The earlier of the two estimated end dates
            togetherEndDate = new Date(Math.min(estimatedEndDate.getTime(), spouseEstimatedEndDate.getTime()));

            retirementDate = new Date(RETIREMENT_YEAR, 0, 1);
        }

        /**
         * Handles the click event for the Save Settings button.
         * Validates inputs, updates global variables, saves settings, and refreshes countdown.
         */
        saveSettingsBtn.addEventListener('click', function() {
            const newBirthMonth = parseInt(birthMonthInput.value);
            const newBirthDay = parseInt(birthDayInput.value);
            const newBirthYear = parseInt(birthYearInput.value);
            const newLifespan = parseInt(lifespanInput.value);

            const newSpouseBirthMonth = parseInt(spouseBirthMonthInput.value);
            const newSpouseBirthDay = parseInt(spouseBirthDayInput.value);
            const newSpouseBirthYear = parseInt(spouseBirthYearInput.value);
            const newSpouseLifespan = parseInt(spouseLifespanInput.value);

            const newRetirementYear = parseInt(retirementYearInput.value);
            const newTvHoursPerDay = parseFloat(tvHoursPerDayInput.value);
            const newSurfSessionsPerWeek = parseFloat(surfSessionsPerWeekInput.value);
            const newVacationsPerYear = parseFloat(vacationsPerYearInput.value); // Get new vacations input

            const currentYear = new Date().getFullYear();

            // --- Validation for User's Birth Date ---
            if (isNaN(newBirthMonth) || newBirthMonth < 1 || newBirthMonth > 12) {
                showInputMessage('Please enter a valid birth month (1-12) for yourself.', 'error'); return; }
            if (isNaN(newBirthDay) || newBirthDay < 1 || newBirthDay > 31) {
                showInputMessage('Please enter a valid birth day (1-31) for yourself.', 'error'); return; }
            if (isNaN(newBirthYear) || newBirthYear < 1900 || newBirthYear > currentYear) {
                showInputMessage(`Please enter a valid birth year (1900-${currentYear}) for yourself.`, 'error'); return; }
            const testUserDate = new Date(newBirthYear, newBirthMonth - 1, newBirthDay);
            if (testUserDate.getMonth() + 1 !== newBirthMonth || testUserDate.getDate() !== newBirthDay || testUserDate.getFullYear() !== newBirthYear) {
                showInputMessage('Please enter a valid birth date for yourself.', 'error'); return; }

            // --- Validation for Spouse's Birth Date ---
            if (isNaN(newSpouseBirthMonth) || newSpouseBirthMonth < 1 || newSpouseBirthMonth > 12) {
                showInputMessage('Please enter a valid birth month (1-12) for your spouse.', 'error'); return; }
            if (isNaN(newSpouseBirthDay) || newSpouseBirthDay < 1 || newSpouseBirthDay > 31) {
                showInputMessage('Please enter a valid birth day (1-31) for your spouse.', 'error'); return; }
            if (isNaN(newSpouseBirthYear) || newSpouseBirthYear < 1900 || newSpouseBirthYear > currentYear) {
                showInputMessage(`Please enter a valid birth year (1900-${currentYear}) for your spouse.`, 'error'); return; }
            const testSpouseDate = new Date(newSpouseBirthYear, newSpouseBirthMonth - 1, newSpouseBirthDay);
            if (testSpouseDate.getMonth() + 1 !== newSpouseBirthMonth || testSpouseDate.getDate() !== newSpouseBirthDay || testSpouseDate.getFullYear() !== newSpouseBirthYear) {
                showInputMessage('Please enter a valid birth date for your spouse.', 'error'); return; }

            // --- Validation for Lifespans ---
            if (isNaN(newLifespan) || newLifespan < 1 || newLifespan > 120) {
                showInputMessage('Please enter a valid lifespan (1-120 years) for yourself.', 'error'); return; }
            if (isNaN(newSpouseLifespan) || newSpouseLifespan < 1 || newSpouseLifespan > 120) {
                showInputMessage('Please enter a valid lifespan (1-120 years) for your spouse.', 'error'); return; }

            // --- Validation for Other Inputs ---
            if (isNaN(newRetirementYear) || newRetirementYear < Math.max(newBirthYear, newSpouseBirthYear) || newRetirementYear > 2100) {
                showInputMessage('Please enter a valid retirement year (after both birth years, up to 2100).', 'error'); return; }
            if (isNaN(newTvHoursPerDay) || newTvHoursPerDay < 0 || newTvHoursPerDay > ACTIVE_HOURS_PER_DAY_BASE) {
                showInputMessage(`TV hours cannot exceed ${ACTIVE_HOURS_PER_DAY_BASE} hours per day.`, 'error'); return; }
            if (isNaN(newSurfSessionsPerWeek) || newSurfSessionsPerWeek < 0 || newSurfSessionsPerWeek > 7) {
                showInputMessage(`Surf sessions per week must be between 0 and 7.`, 'error'); return; }
            if (isNaN(newVacationsPerYear) || newVacationsPerYear < 0 || newVacationsPerYear > 52) {
                showInputMessage(`Vacations per year must be between 0 and 52.`, 'error'); return; }


            // Update global variables
            BIRTH_MONTH = newBirthMonth; BIRTH_DAY = newBirthDay; BIRTH_YEAR = newBirthYear; ESTIMATED_LIFESPAN_YEARS = newLifespan;
            SPOUSE_BIRTH_MONTH = newSpouseBirthMonth; SPOUSE_BIRTH_DAY = newSpouseBirthDay; SPOUSE_BIRTH_YEAR = newSpouseBirthYear; SPOUSE_LIFESPAN_YEARS = newSpouseLifespan;
            RETIREMENT_YEAR = newRetirementYear; TV_HOURS_PER_DAY = newTvHoursPerDay; SURF_SESSIONS_PER_WEEK = newSurfSessionsPerWeek; VACATIONS_PER_YEAR = newVacationsPerYear;

            // Recalculate dates based on new inputs
            recalculateDates();

            // Save settings to localStorage
            saveSettings();

            // Immediately update the countdown display
            updateCountdown();

            showInputMessage('Settings saved successfully!', 'success');
        });

        // Store the interval ID so we can clear it when settings are updated
        let countdownInterval;

        /**
         * Starts or restarts the countdown interval.
         */
        function startCountdown() {
            if (countdownInterval) {
                clearInterval(countdownInterval); // Clear any existing interval
            }
            updateCountdown(); // Initial call
            countdownInterval = setInterval(updateCountdown, 1000); // Start new interval
        }

        /**
         * Calculates the number of a specific recurring event (like a birthday or holiday)
         * between the current date and the estimated end date.
         * @param {number} targetMonth - The month of the event (1-12).
         * @param {number} targetDay - The day of the event (1-31).
         * @param {Date} now - The current date.
         * @param {Date} endDate - The specific end date for this calculation (e.g., estimatedEndDate or togetherEndDate).
         * @returns {number} The count of the event occurrences.
         */
        function countRecurringEvents(targetMonth, targetDay, now, endDate) {
            let count = 0;
            const currentYear = now.getFullYear();
            const endYear = endDate.getFullYear();

            for (let year = currentYear; year <= endYear; year++) {
                const eventDateThisYear = new Date(year, targetMonth - 1, targetDay); // Month is 0-indexed

                // Ensure the date is valid (e.g., handles Feb 29 for non-leap years)
                if (eventDateThisYear.getMonth() + 1 !== targetMonth || eventDateThisYear.getDate() !== targetDay) {
                    continue; // Skip invalid dates like Feb 30th
                }

                // Check if the event falls within the remaining lifespan
                if (eventDateThisYear >= now && eventDateThisYear <= endDate) {
                    count++;
                }
            }
            return count;
        }

        /**
         * Updates the countdown display based on the time remaining until the estimated end date.
         * This function is called repeatedly by setInterval.
         */
        function updateCountdown() {
            const now = new Date(); /* Get the current time */

            // --- User's Individual Countdown ---
            const timeLeftMsUser = estimatedEndDate.getTime() - now.getTime();
            const totalSecondsLeftUser = Math.floor(timeLeftMsUser / 1000);

            // --- Active Hours (User) ---
            let activeSecondsTotalAfterSleepUser = Math.floor(totalSecondsLeftUser * (ACTIVE_HOURS_PER_DAY_BASE / 24));
            const workEndDateForCalculationUser = Math.min(retirementDate.getTime(), estimatedEndDate.getTime());
            const workDurationMsUser = Math.max(0, workEndDateForCalculationUser - now.getTime());
            const workWeeksDurationUser = workDurationMsUser / (1000 * 60 * 60 * 24 * 7);
            const totalWorkHoursToSubtractUser = workWeeksDurationUser * WORK_HOURS_PER_WEEK;
            const totalWorkSecondsToSubtractUser = totalWorkHoursToSubtractUser * 60 * 60;
            let activeSecondsAfterSleepAndWorkUser = Math.max(0, activeSecondsTotalAfterSleepUser - totalWorkSecondsToSubtractUser);

            const activeHoursLeftWithoutTV = Math.floor(activeSecondsAfterSleepAndWorkUser / (60 * 60));

            const totalTVHoursToSubtractUser = (totalSecondsLeftUser / (60 * 60 * 24)) * TV_HOURS_PER_DAY;
            const totalTVSecondsToSubtractUser = totalTVHoursToSubtractUser * 60 * 60;
            let activeSecondsAfterAllDeductionsUser = Math.max(0, activeSecondsAfterSleepAndWorkUser - totalTVSecondsToSubtractUser);
            const activeHoursLeftWithTV = Math.floor(activeSecondsAfterAllDeductionsUser / (60 * 60));

            // --- Individual Milestones (User) ---
            const sunsetsLeft = Math.ceil((estimatedEndDate.getTime() - now.getTime()) / (1000 * 60 * 60 * 24)); // Sunsets include current partial day
            
            const birthdays = countRecurringEvents(BIRTH_MONTH, BIRTH_DAY, now, estimatedEndDate);
            const christmases = countRecurringEvents(12, 25, now, estimatedEndDate);
            const newYears = countRecurringEvents(1, 1, now, estimatedEndDate);
            const surfSessionsTotal = Math.max(0, Math.ceil(((estimatedEndDate.getTime() - now.getTime()) / (1000 * 60 * 60 * 24 * 7)) * SURF_SESSIONS_PER_WEEK));


            // --- Time Left Together Calculations ---
            const timeLeftMsTogether = togetherEndDate.getTime() - now.getTime();

            // If together time is over, set all together counters to 0
            if (timeLeftMsTogether <= 0) {
                togetherDaysLeftElement.textContent = '0';
                togetherActiveHoursLeftElement.textContent = '0';
                morningCoffeeLeftElement.textContent = '0';
                togetherBirthdaysLeftElement.textContent = '0';
                togetherValentinesLeftElement.textContent = '0'; // Set to 0
                vacationsRemainingElement.textContent = '0';
                statusMessageElement.textContent = "Time Together is Up!";
                statusMessageElement.classList.remove('hidden');
                // Don't clear main interval, as individual time might still be left
            } else {
                // Corrected: Use Math.ceil for daysLeftTogether to align with Morning Coffees
                const daysLeftTogether = Math.ceil(timeLeftMsTogether / (1000 * 60 * 60 * 24));

                const totalSecondsLeftTogether = Math.floor(timeLeftMsTogether / 1000); // Still use floor for seconds for other calculations

                // Active Hours Together (excluding sleep, work, TV)
                let activeSecondsTotalAfterSleepTogether = Math.floor(totalSecondsLeftTogether * (ACTIVE_HOURS_PER_DAY_BASE / 24));
                const workEndDateForCalculationTogether = Math.min(retirementDate.getTime(), togetherEndDate.getTime());
                const workDurationMsTogether = Math.max(0, workEndDateForCalculationTogether - now.getTime());
                const workWeeksDurationTogether = workDurationMsTogether / (1000 * 60 * 60 * 24 * 7);
                const totalWorkHoursToSubtractTogether = workWeeksDurationTogether * WORK_HOURS_PER_WEEK;
                const totalWorkSecondsToSubtractTogether = totalWorkHoursToSubtractTogether * 60 * 60;
                let activeSecondsAfterSleepAndWorkTogether = Math.max(0, activeSecondsTotalAfterSleepTogether - totalWorkSecondsToSubtractTogether);

                const totalTVHoursToSubtractTogether = (totalSecondsLeftTogether / (60 * 60 * 24)) * TV_HOURS_PER_DAY;
                const totalTVSecondsToSubtractTogether = totalTVHoursToSubtractTogether * 60 * 60;
                let activeSecondsAfterAllDeductionsTogether = Math.max(0, activeSecondsAfterSleepAndWorkTogether - totalTVSecondsToSubtractTogether);
                const activeHoursLeftTogether = Math.floor(activeSecondsAfterAllDeductionsTogether / (60 * 60));

                // Morning Coffees (already uses Math.ceil)
                const morningCoffeeLeft = Math.ceil(timeLeftMsTogether / (1000 * 60 * 60 * 24));

                // Birthdays (Yours & Spouse's)
                const userBirthdaysTogether = countRecurringEvents(BIRTH_MONTH, BIRTH_DAY, now, togetherEndDate);
                const spouseBirthdaysTogether = countRecurringEvents(SPOUSE_BIRTH_MONTH, SPOUSE_BIRTH_DAY, now, togetherEndDate);
                const totalBirthdaysTogether = userBirthdaysTogether + spouseBirthdaysTogether;

                // Holidays (New Year's, Valentine's Day, Thanksgiving, Christmas)
                // Removed New Year's, Thanksgiving, Christmas from display, only Valentine's remains
                const valentinesTogether = countRecurringEvents(2, 14, now, togetherEndDate);


                // Vacations Remaining
                const yearsLeftTogetherFloat = timeLeftMsTogether / (1000 * 60 * 60 * 24 * 365.25);
                const vacationsRemaining = Math.ceil(yearsLeftTogetherFloat * VACATIONS_PER_YEAR);


                // --- Update Together Display ---
                togetherDaysLeftElement.textContent = daysLeftTogether.toLocaleString();
                togetherActiveHoursLeftElement.textContent = activeHoursLeftTogether.toLocaleString();
                morningCoffeeLeftElement.textContent = morningCoffeeLeft.toLocaleString();
                togetherBirthdaysLeftElement.textContent = totalBirthdaysTogether.toLocaleString();
                togetherValentinesLeftElement.textContent = valentinesTogether.toLocaleString(); // Display
                vacationsRemainingElement.textContent = vacationsRemaining.toLocaleString();
                statusMessageElement.classList.add('hidden');
            }


            // --- Update Individual Display (always runs) ---
            sunsetsLeftElement.textContent = sunsetsLeft.toLocaleString();
            activeHoursLeftWithoutTVElement.textContent = activeHoursLeftWithoutTV.toLocaleString();
            activeHoursLeftWithTVElement.textContent = activeHoursLeftWithTV.toLocaleString();
            birthdaysLeftElement.textContent = birthdays.toLocaleString();
            christmasesLeftElement.textContent = christmases.toLocaleString();
            newYearsLeftElement.textContent = newYears.toLocaleString();
            surfSessionsTotalElement.textContent = surfSessionsTotal.toLocaleString();

            // Handle overall "Time's Up!" if user's estimated end date is reached
            if (timeLeftMsUser <= 0) {
                statusMessageElement.textContent = "Time's Up!";
                statusMessageElement.classList.remove('hidden');
                clearInterval(countdownInterval); // Stop the main interval
            }
        }

        /* --- Initialization --- */
        /* Load settings from localStorage when the page loads */
        loadSettings();
        /* Start the life countdown */
        startCountdown();
    </script>
</body>
</html>
