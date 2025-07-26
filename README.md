<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Digital Life Watch</title>
    <!-- Tailwind CSS CDN for modern styling -->
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
            justify-content: space-between; /* Pushes label to left, value to right */
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
            padding-right: 10px; /* Space between label and value */
        }
        /* The actual countdown numbers - unified with input font size */
        .countdown-value {
            font-family: 'monospace'; /* Using monospace for reliable digital look */
            font-size: 0.95rem; /* Unified with input font size */
            font-weight: 700; /* font-bold in Tailwind */
            color: #63b3ed; /* text-blue-300 in Tailwind */
            text-shadow: 0 0 8px rgba(99, 179, 237, 0.6); /* Subtle glow effect */
            line-height: 1; /* Compact line height */
            text-align: right; /* Ensure value is right-aligned within its space */
            flex-grow: 1; /* Allow value to take up remaining space */
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
        <div class="input-group">
            <label for="retirementYearInput">Target Retirement Year:</label>
            <input type="number" id="retirementYearInput" placeholder="e.g., 2055" min="1900" max="2100">
        </div>
        <div class="input-group">
            <label for="tvHoursPerDayInput">Hours of TV/Streaming per Day:</label>
            <input type="number" id="tvHoursPerDayInput" placeholder="e.g., 2" min="0" max="24">
        </div>
        <button id="saveSettingsBtn" class="save-button">Save Settings</button>
        <div id="inputMessageBox" class="message-box hidden"></div>

        <hr class="w-full border-t border-gray-600 my-8"> <!-- Separator -->

        <!-- Countdown Display Section -->
        <div class="countdown-item">
            <span class="countdown-label">Sunsets Left</span>
            <span id="sunsetsLeft" class="countdown-value">--</span>
        </div>

        <div class="countdown-item">
            <span class="countdown-label">Active Hours Left (Excl. Sleep & Work)</span>
            <span id="activeHoursLeftWithoutTV" class="countdown-value">--</span>
        </div>

        <div class="countdown-item">
            <span class="countdown-label">Active Hours Left (Excl. Sleep, Work & TV)</span>
            <span id="activeHoursLeftWithTV" class="countdown-value">--</span>
        </div>

        <div class="countdown-item">
            <span class="countdown-label">Remaining Birthdays, Christmases, New Year's, Summers</span>
            <span id="annualEventsTotal" class="countdown-value">--</span>
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
        let RETIREMENT_YEAR = 2055;
        let TV_HOURS_PER_DAY = 2; // Default hours of TV/streaming per day
        const ACTIVE_HOURS_PER_DAY_BASE = 16; // 24 hours - 8 hours for sleep (base active hours before work/TV)
        const WORK_HOURS_PER_WEEK = 40; // Hours worked per week

        // --- Date Variables (will be recalculated) ---
        let estimatedEndDate;
        let retirementDate;
        let userBirthDate; // New variable for the precise birth date

        // --- DOM Element References ---
        const birthMonthInput = document.getElementById('birthMonthInput');
        const birthDayInput = document.getElementById('birthDayInput');
        const birthYearInput = document.getElementById('birthYearInput');
        const lifespanInput = document.getElementById('lifespanInput');
        const retirementYearInput = document.getElementById('retirementYearInput');
        const tvHoursPerDayInput = document.getElementById('tvHoursPerDayInput');
        const saveSettingsBtn = document.getElementById('saveSettingsBtn');
        const inputMessageBox = document.getElementById('inputMessageBox');

        const sunsetsLeftElement = document.getElementById('sunsetsLeft');
        const activeHoursLeftWithoutTVElement = document.getElementById('activeHoursLeftWithoutTV');
        const activeHoursLeftWithTVElement = document.getElementById('activeHoursLeftWithTV');
        const annualEventsTotalElement = document.getElementById('annualEventsTotal'); // This will now show only birthdays
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
            const savedRetirementYear = localStorage.getItem('retirementYear');
            const savedTvHoursPerDay = localStorage.getItem('tvHoursPerDay');

            // Load and apply birth date components
            if (savedBirthMonth) {
                BIRTH_MONTH = parseInt(savedBirthMonth);
                birthMonthInput.value = BIRTH_MONTH;
            } else {
                birthMonthInput.value = BIRTH_MONTH; // Set default if not saved
            }
            if (savedBirthDay) {
                BIRTH_DAY = parseInt(savedBirthDay);
                birthDayInput.value = BIRTH_DAY;
            } else {
                birthDayInput.value = BIRTH_DAY; // Set default if not saved
            }
            if (savedBirthYear) {
                BIRTH_YEAR = parseInt(savedBirthYear);
                birthYearInput.value = BIRTH_YEAR;
            } else {
                birthYearInput.value = BIRTH_YEAR; // Set default if not saved
            }

            // Load and apply other settings
            if (savedLifespan) {
                ESTIMATED_LIFESPAN_YEARS = parseInt(savedLifespan);
                lifespanInput.value = ESTIMATED_LIFESPAN_YEARS;
            } else {
                lifespanInput.value = ESTIMATED_LIFESPAN_YEARS; // Set default if not saved
            }
            if (savedRetirementYear) {
                RETIREMENT_YEAR = parseInt(savedRetirementYear);
                retirementYearInput.value = RETIREMENT_YEAR;
            } else {
                retirementYearInput.value = RETIREMENT_YEAR; // Set default if not saved
            }
            if (savedTvHoursPerDay) {
                TV_HOURS_PER_DAY = parseFloat(savedTvHoursPerDay);
                tvHoursPerDayInput.value = TV_HOURS_PER_DAY;
            } else {
                tvHoursPerDayInput.value = TV_HOURS_PER_DAY; // Set default if not saved
            }

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
            localStorage.setItem('retirementYear', RETIREMENT_YEAR);
            localStorage.setItem('tvHoursPerDay', TV_HOURS_PER_DAY);
        }

        /**
         * Recalculates the estimated end date, retirement date, and user's birth date based on current global variables.
         */
        function recalculateDates() {
            // Month is 0-indexed in JavaScript Date object, so subtract 1
            userBirthDate = new Date(BIRTH_YEAR, BIRTH_MONTH - 1, BIRTH_DAY);

            estimatedEndDate = new Date(
                userBirthDate.getFullYear() + ESTIMATED_LIFESPAN_YEARS,
                userBirthDate.getMonth(),
                userBirthDate.getDate()
            );
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
            const newRetirementYear = parseInt(retirementYearInput.value);
            const newTvHoursPerDay = parseFloat(tvHoursPerDayInput.value);

            const currentYear = new Date().getFullYear();

            // Basic validation for birth date
            if (isNaN(newBirthMonth) || newBirthMonth < 1 || newBirthMonth > 12) {
                showInputMessage('Please enter a valid birth month (1-12).', 'error');
                return;
            }
            if (isNaN(newBirthDay) || newBirthDay < 1 || newBirthDay > 31) {
                showInputMessage('Please enter a valid birth day (1-31).', 'error');
                return;
            }
            if (isNaN(newBirthYear) || newBirthYear < 1900 || newBirthYear > currentYear) {
                showInputMessage(`Please enter a valid birth year (1900-${currentYear}).`, 'error');
                return;
            }

            // Validate the full date to ensure it's a real date (e.g., no Feb 30th)
            const testDate = new Date(newBirthYear, newBirthMonth - 1, newBirthDay);
            if (testDate.getMonth() + 1 !== newBirthMonth || testDate.getDate() !== newBirthDay || testDate.getFullYear() !== newBirthYear) {
                showInputMessage('Please enter a valid birth date.', 'error');
                return;
            }

            // Validate lifespan and retirement year
            if (isNaN(newLifespan) || newLifespan < 1 || newLifespan > 120) {
                showInputMessage('Please enter a valid lifespan (1-120 years).', 'error');
                return;
            }
            if (isNaN(newRetirementYear) || newRetirementYear < newBirthYear || newRetirementYear > 2100) {
                showInputMessage('Please enter a valid retirement year (after birth year, up to 2100).', 'error');
                return;
            }

            // Validate TV hours per day
            if (isNaN(newTvHoursPerDay) || newTvHoursPerDay < 0 || newTvHoursPerDay > ACTIVE_HOURS_PER_DAY_BASE) { // Max 24 hours
                showInputMessage(`TV hours cannot exceed 24 hours per day.`, 'error');
                return;
            }

            // Update global variables
            BIRTH_MONTH = newBirthMonth;
            BIRTH_DAY = newBirthDay;
            BIRTH_YEAR = newBirthYear;
            ESTIMATED_LIFESPAN_YEARS = newLifespan;
            RETIREMENT_YEAR = newRetirementYear;
            TV_HOURS_PER_DAY = newTvHoursPerDay;

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
         * @param {Date} estimatedEndDate - The estimated end date of life.
         * @returns {number} The count of the event occurrences.
         */
        function countRecurringEvents(targetMonth, targetDay, now, estimatedEndDate) {
            let count = 0;
            const currentYear = now.getFullYear();
            const endYear = estimatedEndDate.getFullYear();

            for (let year = currentYear; year <= endYear; year++) {
                const eventDateThisYear = new Date(year, targetMonth - 1, targetDay); // Month is 0-indexed

                // Ensure the date is valid (e.g., handles Feb 29 for non-leap years)
                if (eventDateThisYear.getMonth() + 1 !== targetMonth || eventDateThisYear.getDate() !== targetDay) {
                    continue; // Skip invalid dates like Feb 30th
                }

                // Check if the event falls within the remaining lifespan
                if (eventDateThisYear >= now && eventDateThisYear <= estimatedEndDate) {
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
            const now = new Date(); // Get the current time
            const timeLeftMs = estimatedEndDate.getTime() - now.getTime(); // Milliseconds left until end of life

            // Check if time has run out
            if (timeLeftMs <= 0) {
                // If time is up, set all displays to '0' and show the "Time's Up!" message
                sunsetsLeftElement.textContent = '0';
                activeHoursLeftWithoutTVElement.textContent = '0';
                activeHoursLeftWithTVElement.textContent = '0';
                annualEventsTotalElement.textContent = '0'; // Set to 0
                statusMessageElement.textContent = "Time's Up!";
                statusMessageElement.classList.remove('hidden'); // Make message visible
                clearInterval(countdownInterval); // Stop the countdown from updating
                return; // Exit the function
            }

            // --- Calculate total seconds left ---
            let totalSecondsLeft = Math.floor(timeLeftMs / 1000);

            // --- Calculate total active seconds (excluding sleep) ---
            let activeSecondsTotalAfterSleep = Math.floor(totalSecondsLeft * (ACTIVE_HOURS_PER_DAY_BASE / 24));

            // --- Subtract working hours from active seconds ---
            const workEndDateForCalculation = Math.min(retirementDate.getTime(), estimatedEndDate.getTime());
            const workDurationMs = Math.max(0, workEndDateForCalculation - now.getTime());
            const workWeeksDuration = workDurationMs / (1000 * 60 * 60 * 24 * 7);
            const totalWorkHoursToSubtract = workWeeksDuration * WORK_HOURS_PER_WEEK;
            const totalWorkSecondsToSubtract = totalWorkHoursToSubtract * 60 * 60;

            let activeSecondsAfterSleepAndWork = Math.max(0, activeSecondsTotalAfterSleep - totalWorkSecondsToSubtract);

            // --- Calculate Active Hours Left (Excl. Sleep & Work) - BEFORE TV DEDUCTION ---
            const activeHoursLeftWithoutTV = Math.floor(activeSecondsAfterSleepAndWork / (60 * 60));

            // --- Subtract TV hours from active seconds (for the final display) ---
            const totalTVHoursToSubtract = (totalSecondsLeft / (60 * 60 * 24)) * TV_HOURS_PER_DAY;
            const totalTVSecondsToSubtract = totalTVHoursToSubtract * 60 * 60;

            let activeSecondsAfterAllDeductions = Math.max(0, activeSecondsAfterSleepAndWork - totalTVSecondsToSubtract);

            // --- Break down remaining active seconds into days, hours ---
            const daysLeft = Math.floor(totalSecondsLeft / (60 * 60 * 24)); // Still needed for sunsets

            const activeHoursLeftWithTV = Math.floor(activeSecondsAfterAllDeductions / (60 * 60));

            // --- Calculate and Update Event Counts ---
            const sunsetsLeft = daysLeft; // Sunsets are equal to days left
            const birthdays = countRecurringEvents(BIRTH_MONTH, BIRTH_DAY, now, estimatedEndDate);
            // The other annual events are still calculated but not explicitly displayed as separate numbers
            // const christmases = countRecurringEvents(12, 25, now, estimatedEndDate);
            // const newYears = countRecurringEvents(1, 1, now, estimatedEndDate);
            // let summersLeft = 0;
            // const currentYear = now.getFullYear();
            // const endYear = estimatedEndDate.getFullYear();
            // for (let year = currentYear; year <= endYear; year++) {
            //     const summerStart = new Date(year, 5, 1);
            //     const summerEnd = new Date(year, 7, 31);
            //     if ( (summerStart <= estimatedEndDate && summerEnd >= now) ||
            //          (summerStart >= now && summerStart <= estimatedEndDate) ||
            //          (summerEnd >= now && summerEnd <= estimatedEndDate) ) {
            //         summersLeft++;
            //     }
            // }

            // Display only birthdays, but with the comprehensive label
            annualEventsTotalElement.textContent = birthdays.toLocaleString();


            // --- Update the Display ---
            sunsetsLeftElement.textContent = sunsetsLeft.toLocaleString();
            activeHoursLeftWithoutTVElement.textContent = activeHoursLeftWithoutTV.toLocaleString();
            activeHoursLeftWithTVElement.textContent = activeHoursLeftWithTV.toLocaleString();
            statusMessageElement.classList.add('hidden');
        }

        // --- Initialization ---
        // Load settings from localStorage when the page loads
        loadSettings();
        // Start the life countdown
        startCountdown();
    </script>
</body>
</html>
