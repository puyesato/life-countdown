<!DOCTYPE html>
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
            text-align: center;
            max-width: 500px; /* Increased max width to accommodate inputs */
            width: 100%; /* Responsive width */
            border: 2px solid #4a5568; /* Subtle border */
            display: flex;
            flex-direction: column;
            gap: 15px; /* Adjusted spacing between countdown items */
            justify-content: center;
            align-items: center;
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
        .save-button, .llm-button {
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
        .save-button:hover, .llm-button:hover {
            background-color: #3182ce; /* Blue-600 */
            transform: translateY(-2px);
        }
        .save-button:active, .llm-button:active {
            transform: translateY(0);
        }
        .llm-button {
            background-color: #9f7aea; /* Purple for LLM button */
            margin-top: 20px; /* More space above LLM button */
        }
        .llm-button:hover {
            background-color: #805ad5;
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
        .llm-response-box {
            background-color: #4a5568; /* Darker background for response */
            padding: 15px;
            border-radius: 10px;
            margin-top: 15px;
            border: 1px solid #63b3ed;
            color: #e2e8f0;
            font-size: 0.95rem;
            text-align: center;
            min-height: 50px; /* Ensure it has some height even when empty */
            display: flex;
            align-items: center;
            justify-content: center;
            font-style: italic;
        }
        .llm-response-box.hidden {
            display: none;
        }
        .loading-spinner {
            border: 4px solid rgba(255, 255, 255, 0.3);
            border-top: 4px solid #63b3ed;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }


        /* Styling for each individual countdown display item */
        .countdown-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 100%;
        }
        /* Labels for the countdown values (e.g., "Days Left") */
        .countdown-label {
            font-size: 1.1rem; /* Slightly smaller label font on mobile */
            font-weight: 500; /* font-medium in Tailwind */
            color: #cbd5e0; /* text-gray-300 in Tailwind */
            margin-bottom: 5px; /* Adjusted margin */
        }
        /* The actual countdown numbers */
        .countdown-value {
            font-family: 'monospace'; /* Using monospace for reliable digital look */
            font-size: 2.8rem; /* Smaller font size for mobile */
            font-weight: 700; /* font-bold in Tailwind */
            color: #63b3ed; /* text-blue-300 in Tailwind */
            text-shadow: 0 0 8px rgba(99, 179, 237, 0.6); /* Subtle glow effect */
            line-height: 1; /* Compact line height */
        }

        /* Media queries for larger screens (desktop) to revert to larger sizes */
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
            .save-button, .llm-button {
                padding: 12px 25px;
            }
            .message-box {
                padding: 10px 15px;
                font-size: 0.9rem;
            }
            .countdown-label {
                font-size: 1.25rem;
            }
            .countdown-value {
                font-size: 3.5rem;
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
            <span class="countdown-label">Days Left</span>
            <span id="daysLeft" class="countdown-value">--</span>
        </div>

        <div class="countdown-item">
            <span class="countdown-label">Active Hours Left (Excl. Sleep, Work & TV)</span>
            <span id="activeHoursLeft" class="countdown-value">--</span>
        </div>

        <div class="countdown-item">
            <span class="countdown-label">Current Bitcoin Price (USD)</span>
            <span id="bitcoinPrice" class="countdown-value">Loading...</span>
        </div>

        <div class="countdown-item">
            <span class="countdown-label">Current Bitcoin Block Height</span>
            <span id="blockHeight" class="countdown-value">Loading...</span>
        </div>

        <!-- LLM Feature Section -->
        <button id="getReflectionBtn" class="llm-button">Get a Life Reflection ✨</button>
        <div id="reflectionResponse" class="llm-response-box hidden">
            <div class="loading-spinner hidden" id="reflectionSpinner"></div>
            <span id="reflectionText"></span>
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

        const daysLeftElement = document.getElementById('daysLeft');
        const activeHoursLeftElement = document.getElementById('activeHoursLeft');
        const statusMessageElement = document.getElementById('statusMessage');

        // LLM Feature Elements
        const getReflectionBtn = document.getElementById('getReflectionBtn');
        const reflectionResponseBox = document.getElementById('reflectionResponse');
        const reflectionText = document.getElementById('reflectionText');
        const reflectionSpinner = document.getElementById('reflectionSpinner');

        // Bitcoin Data Elements
        const bitcoinPriceElement = document.getElementById('bitcoinPrice');
        const blockHeightElement = document.getElementById('blockHeight');

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
        let bitcoinDataInterval; // New interval for Bitcoin data

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
         * Updates the countdown display based on the time remaining until the estimated end date.
         * This function is called repeatedly by setInterval.
         */
        function updateCountdown() {
            const now = new Date(); // Get the current time
            const timeLeftMs = estimatedEndDate.getTime() - now.getTime(); // Milliseconds left until end of life

            // Check if time has run out
            if (timeLeftMs <= 0) {
                // If time is up, set all displays to '0' and show the "Time's Up!" message
                daysLeftElement.textContent = '0';
                activeHoursLeftElement.textContent = '0';
                statusMessageElement.textContent = "Time's Up!";
                statusMessageElement.classList.remove('hidden'); // Make message visible
                clearInterval(countdownInterval); // Stop the countdown from updating
                return; // Exit the function
            }

            // --- Calculate total seconds left ---
            let totalSecondsLeft = Math.floor(timeLeftMs / 1000);

            // --- Calculate total active seconds (excluding sleep) ---
            // This is the total time available for non-sleep activities across the entire remaining lifespan
            let activeSecondsTotal = Math.floor(totalSecondsLeft * (ACTIVE_HOURS_PER_DAY_BASE / 24));

            // --- Subtract working hours from active seconds ---
            // Determine the end of the working period for this calculation.
            // It's either the retirement date or the estimated end of life, whichever comes first.
            const workEndDateForCalculation = Math.min(retirementDate.getTime(), estimatedEndDate.getTime());

            // Calculate the duration of the working period from now until the determined end date.
            const workDurationMs = Math.max(0, workEndDateForCalculation - now.getTime());

            // Convert the working duration from milliseconds to weeks.
            const workWeeksDuration = workDurationMs / (1000 * 60 * 60 * 24 * 7);

            // Calculate the total work hours to subtract over this period.
            const totalWorkHoursToSubtract = workWeeksDuration * WORK_HOURS_PER_WEEK;
            // Convert total work hours to seconds.
            const totalWorkSecondsToSubtract = totalWorkHoursToSubtract * 60 * 60;

            // Subtract the calculated work seconds from the total active seconds.
            activeSecondsTotal = Math.max(0, activeSecondsTotal - totalWorkSecondsToSubtract);

            // --- Subtract TV hours from active seconds ---
            // Calculate total TV hours to subtract over the entire remaining lifespan (until estimatedEndDate)
            const totalTVHoursToSubtract = (totalSecondsLeft / (60 * 60 * 24)) * TV_HOURS_PER_DAY;
            const totalTVSecondsToSubtract = totalTVHoursToSubtract * 60 * 60;

            // Subtract TV seconds from the remaining active seconds.
            activeSecondsTotal = Math.max(0, activeSecondsTotal - totalTVSecondsToSubtract);


            // --- Break down remaining active seconds into days, hours ---
            // Days left is still based on the *total* time remaining, not just active time.
            const daysLeft = Math.floor(totalSecondsLeft / (60 * 60 * 24));

            // Calculate active hours from the remaining active seconds
            const activeHoursLeft = Math.floor(activeSecondsTotal / (60 * 60));

            // --- Update the Display ---
            daysLeftElement.textContent = daysLeft.toLocaleString();
            activeHoursLeftElement.textContent = activeHoursLeft.toLocaleString();
            statusMessageElement.classList.add('hidden');
        }

        /**
         * Fetches current Bitcoin price and block height from external APIs.
         */
        async function fetchBitcoinData() {
            bitcoinPriceElement.textContent = 'Loading...';
            blockHeightElement.textContent = 'Loading...';

            try {
                // Fetch Bitcoin Price from CoinGecko
                const priceResponse = await fetch('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=usd');
                const priceData = await priceResponse.json();
                if (priceData && priceData.bitcoin && priceData.bitcoin.usd) {
                    bitcoinPriceElement.textContent = `$${priceData.bitcoin.usd.toLocaleString()}`;
                } else {
                    bitcoinPriceElement.textContent = 'N/A';
                    console.error('Error fetching Bitcoin price: Unexpected data structure', priceData);
                }
            } catch (error) {
                bitcoinPriceElement.textContent = 'Error';
                console.error('Error fetching Bitcoin price:', error);
            }

            try {
                // Fetch Bitcoin Block Height from Blockchain.com
                const blockHeightResponse = await fetch('https://blockchain.info/q/getblockcount');
                const blockHeightData = await blockHeightResponse.text(); // block height is plain text
                const blockHeight = parseInt(blockHeightData);
                if (!isNaN(blockHeight)) {
                    blockHeightElement.textContent = blockHeight.toLocaleString();
                } else {
                    blockHeightElement.textContent = 'N/A';
                    console.error('Error fetching block height: Unexpected data', blockHeightData);
                }
            } catch (error) {
                blockHeightElement.textContent = 'Error';
                console.error('Error fetching block height:', error);
            }
        }


        /**
         * Calls the Gemini API to get a life reflection prompt.
         */
        getReflectionBtn.addEventListener('click', async function() {
            reflectionResponseBox.classList.remove('hidden');
            reflectionText.textContent = ''; // Clear previous text
            reflectionSpinner.classList.remove('hidden'); // Show spinner

            const prompt = "You are a thoughtful life coach. Given that the user is reflecting on their life's duration, provide a single, concise, and inspiring question or thought for self-reflection. Focus on making the most of their time, personal growth, or pursuing passions. Do not mention specific numbers or calculations from the app. Keep it under 50 words.";

            let chatHistory = [];
            chatHistory.push({ role: "user", parts: [{ text: prompt }] });

            const payload = { contents: chatHistory };
            const apiKey = ""; // Canvas will automatically provide this in runtime
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();

                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    reflectionText.textContent = text;
                } else {
                    reflectionText.textContent = "Could not generate a reflection. Please try again.";
                    console.error("Gemini API response structure unexpected:", result);
                }
            } catch (error) {
                reflectionText.textContent = "Error fetching reflection. Please check your connection or try again.";
                console.error("Error calling Gemini API:", error);
            } finally {
                reflectionSpinner.classList.add('hidden'); // Hide spinner
            }
        });


        // --- Initialization ---
        // Load settings from localStorage when the page loads
        loadSettings();
        // Start the life countdown
        startCountdown();
        // Fetch Bitcoin data immediately and then every 30 seconds
        fetchBitcoinData();
        bitcoinDataInterval = setInterval(fetchBitcoinData, 30000); // Update every 30 seconds
    </script>
</body>
</html>
