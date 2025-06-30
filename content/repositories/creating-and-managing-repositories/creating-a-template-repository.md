<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FitQuest - Ultimate Fitness Assessment Game</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        .gradient-bg {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        }
        .game-card {
            backdrop-filter: blur(10px);
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        .progress-bar {
            transition: width 0.5s ease-in-out;
        }
        .bounce-in {
            animation: bounceIn 0.6s ease-out;
        }
        @keyframes bounceIn {
            0% { transform: scale(0.3); opacity: 0; }
            50% { transform: scale(1.05); }
            70% { transform: scale(0.9); }
            100% { transform: scale(1); opacity: 1; }
        }
        .pulse-effect {
            animation: pulse 2s infinite;
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
    </style>
</head>
<body class="gradient-bg min-h-screen text-white">
    <div class="container mx-auto px-4 py-8">
        <!-- Welcome Screen -->
        <div id="welcome-screen" class="text-center">
            <div class="bounce-in">
                <i class="fas fa-dumbbell text-8xl mb-8 text-yellow-400"></i>
                <h1 class="text-6xl font-bold mb-4">FitQuest</h1>
                <p class="text-2xl mb-8">Ultimate Fitness Assessment Game</p>
                <p class="text-lg mb-12 max-w-2xl mx-auto">
                    Discover your fitness level, get personalized diet plans, and receive custom workout routines 
                    based on your goals and lifestyle. Let's unlock your fitness potential!
                </p>
                <button onclick="startAssessment()" class="bg-yellow-500 hover:bg-yellow-600 text-black font-bold py-4 px-8 rounded-full text-xl transition-all duration-300 transform hover:scale-105 pulse-effect">
                    <i class="fas fa-play mr-2"></i>Start Your Quest
                </button>
            </div>
        </div>

        <!-- Assessment Form -->
        <div id="assessment-form" class="hidden">
            <div class="max-w-4xl mx-auto">
                <div class="bg-white bg-opacity-10 rounded-lg p-8 backdrop-filter backdrop-blur-lg">
                    <div class="mb-8">
                        <div class="flex justify-between items-center mb-4">
                            <h2 class="text-3xl font-bold">Physical Assessment</h2>
                            <span class="text-lg" id="progress-text">Step 1 of 4</span>
                        </div>
                        <div class="w-full bg-gray-300 rounded-full h-3">
                            <div id="progress-bar" class="bg-yellow-500 h-3 rounded-full progress-bar" style="width: 25%"></div>
                        </div>
                    </div>

                    <!-- Step 1: Basic Info -->
                    <div id="step-1" class="assessment-step">
                        <h3 class="text-2xl font-semibold mb-6"><i class="fas fa-user mr-2"></i>Basic Information</h3>
                        <div class="grid md:grid-cols-2 gap-6">
                            <div>
                                <label class="block text-lg font-medium mb-2">Age</label>
                                <input type="number" id="age" class="w-full p-3 rounded-lg text-black" placeholder="Enter your age" min="16" max="100">
                            </div>
                            <div>
                                <label class="block text-lg font-medium mb-2">Gender</label>
                                <select id="gender" class="w-full p-3 rounded-lg text-black">
                                    <option value="">Select Gender</option>
                                    <option value="male">Male</option>
                                    <option value="female">Female</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-lg font-medium mb-2">Height (cm)</label>
                                <input type="number" id="height" class="w-full p-3 rounded-lg text-black" placeholder="Enter height in cm" min="100" max="250">
                            </div>
                            <div>
                                <label class="block text-lg font-medium mb-2">Weight (kg)</label>
                                <input type="number" id="weight" class="w-full p-3 rounded-lg text-black" placeholder="Enter weight in kg" min="30" max="300">
                            </div>
                        </div>
                    </div>

                    <!-- Step 2: Fitness Tests -->
                    <div id="step-2" class="assessment-step hidden">
                        <h3 class="text-2xl font-semibold mb-6"><i class="fas fa-heartbeat mr-2"></i>Fitness Tests</h3>
                        <div class="grid md:grid-cols-2 gap-6">
                            <div>
                                <label class="block text-lg font-medium mb-2">Maximum Push-ups</label>
                                <input type="number" id="pushups" class="w-full p-3 rounded-lg text-black" placeholder="How many push-ups can you do?" min="0" max="200">
                            </div>
                            <div>
                                <label class="block text-lg font-medium mb-2">1.5 Mile Run Time (minutes)</label>
                                <input type="number" id="run-time" step="0.5" class="w-full p-3 rounded-lg text-black" placeholder="Time to run 1.5 miles" min="5" max="30">
                            </div>
                            <div>
                                <label class="block text-lg font-medium mb-2">Resting Heart Rate (BPM)</label>
                                <input type="number" id="heart-rate" class="w-full p-3 rounded-lg text-black" placeholder="Resting heart rate" min="40" max="120">
                            </div>
                            <div>
                                <label class="block text-lg font-medium mb-2">Sit-and-Reach (cm)</label>
                                <input type="number" id="flexibility" class="w-full p-3 rounded-lg text-black" placeholder="Flexibility test result" min="-20" max="50">
                            </div>
                        </div>
                    </div>

                    <!-- Step 3: Lifestyle -->
                    <div id="step-3" class="assessment-step hidden">
                        <h3 class="text-2xl font-semibold mb-6"><i class="fas fa-briefcase mr-2"></i>Lifestyle & Goals</h3>
                        <div class="space-y-6">
                            <div>
                                <label class="block text-lg font-medium mb-2">Profession</label>
                                <select id="profession" class="w-full p-3 rounded-lg text-black">
                                    <option value="">Select your profession</option>
                                    <option value="desk-job">Desk Job / Office Worker</option>
                                    <option value="manual-labor">Manual Labor / Construction</option>
                                    <option value="healthcare">Healthcare Worker</option>
                                    <option value="teacher">Teacher / Educator</option>
                                    <option value="retail">Retail / Service Industry</option>
                                    <option value="student">Student</option>
                                    <option value="retired">Retired</option>
                                    <option value="athlete">Athlete / Fitness Professional</option>
                                    <option value="other">Other</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-lg font-medium mb-2">Primary Fitness Goal</label>
                                <select id="goal" class="w-full p-3 rounded-lg text-black">
                                    <option value="">Select your goal</option>
                                    <option value="weight-loss">Weight Loss</option>
                                    <option value="muscle-gain">Muscle Gain</option>
                                    <option value="endurance">Improve Endurance</option>
                                    <option value="strength">Build Strength</option>
                                    <option value="general-health">General Health</option>
                                    <option value="flexibility">Improve Flexibility</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-lg font-medium mb-2">Current Activity Level</label>
                                <select id="activity-level" class="w-full p-3 rounded-lg text-black">
                                    <option value="">Select activity level</option>
                                    <option value="sedentary">Sedentary (little to no exercise)</option>
                                    <option value="light">Light (1-3 days/week)</option>
                                    <option value="moderate">Moderate (3-5 days/week)</option>
                                    <option value="active">Active (6-7 days/week)</option>
                                    <option value="very-active">Very Active (2x/day or intense training)</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <!-- Step 4: Preferences -->
                    <div id="step-4" class="assessment-step hidden">
                        <h3 class="text-2xl font-semibold mb-6"><i class="fas fa-cog mr-2"></i>Workout Preferences</h3>
                        <div class="space-y-6">
                            <div>
                                <label class="block text-lg font-medium mb-2">Do you have gym access?</label>
                                <div class="flex space-x-4">
                                    <label class="flex items-center">
                                        <input type="radio" name="gym-access" value="yes" class="mr-2">
                                        <span>Yes, I go to a gym</span>
                                    </label>
                                    <label class="flex items-center">
                                        <input type="radio" name="gym-access" value="no" class="mr-2">
                                        <span>No, home workouts only</span>
                                    </label>
                                </div>
                            </div>
                            <div>
                                <label class="block text-lg font-medium mb-2">Available workout time per day</label>
                                <select id="workout-time" class="w-full p-3 rounded-lg text-black">
                                    <option value="">Select time</option>
                                    <option value="15-30">15-30 minutes</option>
                                    <option value="30-45">30-45 minutes</option>
                                    <option value="45-60">45-60 minutes</option>
                                    <option value="60+">60+ minutes</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-lg font-medium mb-2">Workout days per week</label>
                                <select id="workout-days" class="w-full p-3 rounded-lg text-black">
                                    <option value="">Select days</option>
                                    <option value="2-3">2-3 days</option>
                                    <option value="3-4">3-4 days</option>
                                    <option value="4-5">4-5 days</option>
                                    <option value="5-6">5-6 days</option>
                                    <option value="daily">Daily</option>
                                </select>
                            </div>
                        </div>
                    </div>

                    <!-- Navigation Buttons -->
                    <div class="mt-8 flex justify-between">
                        <button id="prev-btn" onclick="previousStep()" class="bg-gray-500 hover:bg-gray-600 px-6 py-3 rounded-lg font-semibold hidden transition-colors">
                            <i class="fas fa-arrow-left mr-2"></i>Previous
                        </button>
                        <button id="next-btn" onclick="nextStep()" class="bg-blue-500 hover:bg-blue-600 px-6 py-3 rounded-lg font-semibold ml-auto transition-colors">
                            Next<i class="fas fa-arrow-right ml-2"></i>
                        </button>
                        <button id="calculate-btn" onclick="calculateResults()" class="bg-green-500 hover:bg-green-600 px-6 py-3 rounded-lg font-semibold hidden ml-auto transition-colors">
                            <i class="fas fa-calculator mr-2"></i>Calculate Results
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Results Screen -->
        <div id="results-screen" class="hidden">
            <div class="max-w-6xl mx-auto">
                <div class="text-center mb-8">
                    <h2 class="text-4xl font-bold mb-4"><i class="fas fa-trophy text-yellow-400 mr-2"></i>Your FitQuest Results</h2>
                    <p class="text-xl">Congratulations! Here's your comprehensive fitness assessment.</p>
                </div>

                <!-- Results Cards -->
                <div class="grid md:grid-cols-3 gap-6 mb-8">
                    <div class="game-card rounded-lg p-6 text-center">
                        <i class="fas fa-battery-three-quarters text-4xl text-green-400 mb-4"></i>
                        <h3 class="text-2xl font-bold mb-2">Energy Level</h3>
                        <div id="energy-level" class="text-3xl font-bold text-green-400"></div>
                        <p id="energy-description" class="mt-2 text-sm"></p>
                    </div>
                    <div class="game-card rounded-lg p-6 text-center">
                        <i class="fas fa-chart-line text-4xl text-blue-400 mb-4"></i>
                        <h3 class="text-2xl font-bold mb-2">Fitness Age</h3>
                        <div id="fitness-age" class="text-3xl font-bold text-blue-400"></div>
                        <p id="age-description" class="mt-2 text-sm"></p>
                    </div>
                    <div class="game-card rounded-lg p-6 text-center">
                        <i class="fas fa-percentage text-4xl text-purple-400 mb-4"></i>
                        <h3 class="text-2xl font-bold mb-2">Success Rate</h3>
                        <div id="success-rate" class="text-3xl font-bold text-purple-400"></div>
                        <p id="success-description" class="mt-2 text-sm"></p>
                    </div>
                </div>

                <!-- Detailed Charts -->
                <div class="grid md:grid-cols-2 gap-6 mb-8">
                    <div class="game-card rounded-lg p-6">
                        <h3 class="text-xl font-bold mb-4"><i class="fas fa-chart-radar mr-2"></i>Fitness Breakdown</h3>
                        <canvas id="fitness-chart" style="height: 300px;"></canvas>
                    </div>
                    <div class="game-card rounded-lg p-6">
                        <h3 class="text-xl font-bold mb-4"><i class="fas fa-medal mr-2"></i>Achievement Level</h3>
                        <div id="achievement-level" class="text-center">
                            <div id="level-badge" class="text-6xl mb-4"></div>
                            <div id="level-name" class="text-2xl font-bold mb-2"></div>
                            <div id="level-description" class="text-sm"></div>
                        </div>
                    </div>
                </div>

                <!-- Diet Recommendations -->
                <div class="game-card rounded-lg p-6 mb-8">
                    <h3 class="text-2xl font-bold mb-4"><i class="fas fa-utensils mr-2"></i>Personalized Diet Plan</h3>
                    <div id="diet-recommendations" class="grid md:grid-cols-2 gap-6"></div>
                </div>

                <!-- Workout Plan -->
                <div class="game-card rounded-lg p-6 mb-8">
                    <h3 class="text-2xl font-bold mb-4"><i class="fas fa-dumbbell mr-2"></i>Your Workout Plan</h3>
                    <div id="workout-plan"></div>
                </div>

                <!-- Action Buttons -->
                <div class="text-center space-x-4">
                    <button onclick="restartAssessment()" class="bg-blue-500 hover:bg-blue-600 px-6 py-3 rounded-lg font-semibold transition-colors">
                        <i class="fas fa-redo mr-2"></i>Retake Assessment
                    </button>
                    <button onclick="shareResults()" class="bg-green-500 hover:bg-green-600 px-6 py-3 rounded-lg font-semibold transition-colors">
                        <i class="fas fa-share mr-2"></i>Share Results
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        let currentStep = 1;
        let userProfile = {};

        // Fitness standards data
        const fitnessStandards = {
            pushups: {
                male: {
                    25: { excellent: 47, good: 39, average: 30, poor: 20 },
                    35: { excellent: 41, good: 34, average: 25, poor: 17 },
                    45: { excellent: 34, good: 28, average: 21, poor: 14 },
                    55: { excellent: 28, good: 23, average: 17, poor: 11 },
                    65: { excellent: 23, good: 19, average: 14, poor: 9 }
                },
                female: {
                    25: { excellent: 36, good: 30, average: 22, poor: 15 },
                    35: { excellent: 31, good: 26, average: 19, poor: 13 },
                    45: { excellent: 25, good: 21, average: 15, poor: 10 },
                    55: { excellent: 20, good: 16, average: 12, poor: 8 },
                    65: { excellent: 15, good: 12, average: 9, poor: 6 }
                }
            },
            runtime: {
                male: {
                    25: { excellent: 9.5, good: 11.0, average: 12.5, poor: 15.0 },
                    35: { excellent: 10.0, good: 11.5, average: 13.0, poor: 15.5 },
                    45: { excellent: 10.5, good: 12.0, average: 13.5, poor: 16.0 },
                    55: { excellent: 11.5, good: 13.0, average: 14.5, poor: 17.0 },
                    65: { excellent: 12.5, good: 14.0, average: 15.5, poor: 18.0 }
                },
                female: {
                    25: { excellent: 11.5, good: 13.0, average: 14.5, poor: 17.0 },
                    35: { excellent: 12.0, good: 13.5, average: 15.0, poor: 17.5 },
                    45: { excellent: 12.5, good: 14.0, average: 15.5, poor: 18.0 },
                    55: { excellent: 14.0, good: 15.5, average: 17.0, poor: 19.5 },
                    65: { excellent: 15.5, good: 17.0, average: 18.5, poor: 21.0 }
                }
            }
        };

        function startAssessment() {
            document.getElementById('welcome-screen').classList.add('hidden');
            document.getElementById('assessment-form').classList.remove('hidden');
        }

        function nextStep() {
            if (validateCurrentStep()) {
                document.getElementById(`step-${currentStep}`).classList.add('hidden');
                currentStep++;
                document.getElementById(`step-${currentStep}`).classList.remove('hidden');
                updateProgress();
                updateButtons();
            }
        }

        function previousStep() {
            document.getElementById(`step-${currentStep}`).classList.add('hidden');
            currentStep--;
            document.getElementById(`step-${currentStep}`).classList.remove('hidden');
            updateProgress();
            updateButtons();
        }

        function validateCurrentStep() {
            const requiredFields = {
                1: ['age', 'gender', 'height', 'weight'],
                2: ['pushups', 'run-time', 'heart-rate', 'flexibility'],
                3: ['profession', 'goal', 'activity-level'],
                4: ['workout-time', 'workout-days']
            };

            const fields = requiredFields[currentStep];
            for (let field of fields) {
                const element = document.getElementById(field);
                if (!element.value || element.value === '') {
                    alert(`Please fill in the ${field.replace('-', ' ')} field.`);
                    return false;
                }
            }

            // Special validation for gym access radio buttons
            if (currentStep === 4) {
                const gymAccess = document.querySelector('input[name="gym-access"]:checked');
                if (!gymAccess) {
                    alert('Please select your gym access preference.');
                    return false;
                }
            }

            return true;
        }

        function updateProgress() {
            const progress = (currentStep / 4) * 100;
            document.getElementById('progress-bar').style.width = `${progress}%`;
            document.getElementById('progress-text').textContent = `Step ${currentStep} of 4`;
        }

        function updateButtons() {
            const prevBtn = document.getElementById('prev-btn');
            const nextBtn = document.getElementById('next-btn');
            const calculateBtn = document.getElementById('calculate-btn');

            if (currentStep === 1) {
                prevBtn.classList.add('hidden');
            } else {
                prevBtn.classList.remove('hidden');
            }

            if (currentStep === 4) {
                nextBtn.classList.add('hidden');
                calculateBtn.classList.remove('hidden');
            } else {
                nextBtn.classList.remove('hidden');
                calculateBtn.classList.add('hidden');
            }
        }

        function calculateResults() {
            if (!validateCurrentStep()) return;

            // Gather user data
            userProfile = {
                age: parseInt(document.getElementById('age').value),
                gender: document.getElementById('gender').value,
                height: parseInt(document.getElementById('height').value),
                weight: parseInt(document.getElementById('weight').value),
                pushups: parseInt(document.getElementById('pushups').value),
                runTime: parseFloat(document.getElementById('run-time').value),
                heartRate: parseInt(document.getElementById('heart-rate').value),
                flexibility: parseInt(document.getElementById('flexibility').value),
                profession: document.getElementById('profession').value,
                goal: document.getElementById('goal').value,
                activityLevel: document.getElementById('activity-level').value,
                gymAccess: document.querySelector('input[name="gym-access"]:checked').value,
                workoutTime: document.getElementById('workout-time').value,
                workoutDays: document.getElementById('workout-days').value
            };

            // Calculate fitness metrics
            const results = calculateFitnessMetrics(userProfile);
            
            // Display results
            displayResults(results);
            
            // Hide form and show results
            document.getElementById('assessment-form').classList.add('hidden');
            document.getElementById('results-screen').classList.remove('hidden');
        }

        function calculateFitnessMetrics(profile) {
            // Calculate BMI
            const bmi = profile.weight / ((profile.height / 100) ** 2);
            
            // Get age group for standards
            const ageGroup = getAgeGroup(profile.age);
            
            // Calculate fitness scores
            const pushupsScore = calculateScore(profile.pushups, fitnessStandards.pushups[profile.gender][ageGroup]);
            const runScore = calculateScore(profile.runTime, fitnessStandards.runtime[profile.gender][ageGroup], true);
            const heartRateScore = calculateHeartRateScore(profile.heartRate, profile.age);
            const flexibilityScore = calculateFlexibilityScore(profile.flexibility);
            
            // Calculate overall fitness level
            const overallScore = (pushupsScore + runScore + heartRateScore + flexibilityScore) / 4;
            
            // Calculate energy level (1-10)
            const energyLevel = Math.round((overallScore / 100) * 10);
            
            // Calculate fitness age
            const fitnessAge = calculateFitnessAge(profile.age, overallScore);
            
            // Calculate success rate
            const successRate = Math.round(overallScore);
            
            return {
                bmi,
                energyLevel,
                fitnessAge,
                successRate,
                overallScore,
                scores: {
                    strength: pushupsScore,
                    cardio: runScore,
                    heartHealth: heartRateScore,
                    flexibility: flexibilityScore
                }
            };
        }

        function getAgeGroup(age) {
            if (age < 30) return 25;
            if (age < 40) return 35;
            if (age < 50) return 45;
            if (age < 60) return 55;
            return 65;
        }

        function calculateScore(value, standards, isTime = false) {
            if (isTime) {
                // For time-based tests, lower is better
                if (value <= standards.excellent) return 95;
                if (value <= standards.good) return 80;
                if (value <= standards.average) return 65;
                if (value <= standards.poor) return 45;
                return 25;
            } else {
                // For count-based tests, higher is better
                if (value >= standards.excellent) return 95;
                if (value >= standards.good) return 80;
                if (value >= standards.average) return 65;
                if (value >= standards.poor) return 45;
                return 25;
            }
        }

        function calculateHeartRateScore(heartRate, age) {
            const maxHR = 220 - age;
            const restingHRPercent = (heartRate / maxHR) * 100;
            
            if (restingHRPercent < 30) return 95;
            if (restingHRPercent < 35) return 80;
            if (restingHRPercent < 40) return 65;
            if (restingHRPercent < 45) return 45;
            return 25;
        }

        function calculateFlexibilityScore(flexibility) {
            if (flexibility >= 20) return 95;
            if (flexibility >= 10) return 80;
            if (flexibility >= 0) return 65;
            if (flexibility >= -10) return 45;
            return 25;
        }

        function calculateFitnessAge(actualAge, overallScore) {
            const scoreDiff = (overallScore - 65) / 35; // Normalize around average
            return Math.max(18, Math.round(actualAge - (scoreDiff * 10)));
        }

        function displayResults(results) {
            // Energy Level
            document.getElementById('energy-level').textContent = `${results.energyLevel}/10`;
            document.getElementById('energy-description').textContent = getEnergyDescription(results.energyLevel);
            
            // Fitness Age
            document.getElementById('fitness-age').textContent = `${results.fitnessAge} years`;
            document.getElementById('age-description').textContent = getFitnessAgeDescription(userProfile.age, results.fitnessAge);
            
            // Success Rate
            document.getElementById('success-rate').textContent = `${results.successRate}%`;
            document.getElementById('success-description').textContent = getSuccessDescription(results.successRate);
            
            // Achievement Level
            const achievement = getAchievementLevel(results.overallScore);
            document.getElementById('level-badge').textContent = achievement.badge;
            document.getElementById('level-name').textContent = achievement.name;
            document.getElementById('level-description').textContent = achievement.description;
            
            // Fitness Chart
            createFitnessChart(results.scores);
            
            // Diet Recommendations
            displayDietRecommendations();
            
            // Workout Plan
            displayWorkoutPlan();
        }

        function getEnergyDescription(level) {
            if (level >= 8) return "Excellent energy levels! You're a fitness powerhouse.";
            if (level >= 6) return "Good energy levels with room for improvement.";
            if (level >= 4) return "Moderate energy levels. Focus on cardio improvement.";
            return "Low energy levels. Start with basic fitness activities.";
        }

        function getFitnessAgeDescription(actualAge, fitnessAge) {
            const diff = actualAge - fitnessAge;
            if (diff > 10) return "Amazing! You're aging like fine wine.";
            if (diff > 5) return "Great! You're fitter than your peers.";
            if (diff > 0) return "Good! You're in decent shape.";
            if (diff > -5) return "Room for improvement in your fitness.";
            return "Time to focus on your health and fitness.";
        }

        function getSuccessDescription(rate) {
            if (rate >= 85) return "Outstanding fitness performance!";
            if (rate >= 70) return "Good fitness levels achieved.";
            if (rate >= 55) return "Average fitness with potential to improve.";
            return "Below average - let's build your fitness foundation.";
        }

        function getAchievementLevel(score) {
            if (score >= 90) return { badge: "🏆", name: "Fitness Champion", description: "You're in the top tier of fitness!" };
            if (score >= 80) return { badge: "🥇", name: "Fitness Hero", description: "Excellent fitness levels achieved!" };
            if (score >= 70) return { badge: "🥈", name: "Fitness Warrior", description: "Good fitness with room to excel!" };
            if (score >= 60) return { badge: "🥉", name: "Fitness Rookie", description: "On your way to better fitness!" };
            return { badge: "🎯", name: "Fitness Beginner", description: "Every expert was once a beginner!" };
        }

        function createFitnessChart(scores) {
            const ctx = document.getElementById('fitness-chart').getContext('2d');
            new Chart(ctx, {
                type: 'radar',
                data: {
                    labels: ['Strength', 'Cardio', 'Heart Health', 'Flexibility'],
                    datasets: [{
                        label: 'Your Fitness',
                        data: [scores.strength, scores.cardio, scores.heartHealth, scores.flexibility],
                        backgroundColor: 'rgba(255, 206, 84, 0.2)',
                        borderColor: 'rgba(255, 206, 84, 1)',
                        borderWidth: 2,
                        pointBackgroundColor: 'rgba(255, 206, 84, 1)',
                        pointBorderColor: '#fff',
                        pointHoverBackgroundColor: '#fff',
                        pointHoverBorderColor: 'rgba(255, 206, 84, 1)'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        r: {
                            angleLines: {
                                display: true,
                                color: 'rgba(255, 255, 255, 0.1)'
                            },
                            suggestedMin: 0,
                            suggestedMax: 100,
                            ticks: {
                                color: 'white',
                                backdropColor: 'transparent'
                            },
                            pointLabels: {
                                color: 'white',
                                font: {
                                    size: 12
                                }
                            },
                            grid: {
                                color: 'rgba(255, 255, 255, 0.1)'
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            labels: {
                                color: 'white'
                            }
                        }
                    }
                }
            });
        }

        function displayDietRecommendations() {
            const dietPlan = generateDietPlan(userProfile);
            const container = document.getElementById('diet-recommendations');
            
            container.innerHTML = `
                <div class="space-y-4">
                    <div class="bg-white bg-opacity-10 rounded-lg p-4">
                        <h4 class="font-bold text-lg mb-2"><i class="fas fa-bullseye mr-2"></i>Daily Calorie Target</h4>
                        <p class="text-2xl font-bold text-yellow-400">${dietPlan.calories} kcal</p>
                        <p class="text-sm mt-1">${dietPlan.calorieNote}</p>
                    </div>
                    
                    <div class="bg-white bg-opacity-10 rounded-lg p-4">
                        <h4 class="font-bold text-lg mb-2"><i class="fas fa-chart-pie mr-2"></i>Macro Breakdown</h4>
                        <div class="grid grid-cols-3 gap-2 text-center">
                            <div>
                                <div class="text-xl font-bold text-red-400">${dietPlan.macros.protein}g</div>
                                <div class="text-sm">Protein</div>
                            </div>
                            <div>
                                <div class="text-xl font-bold text-blue-400">${dietPlan.macros.carbs}g</div>
                                <div class="text-sm">Carbs</div>
                            </div>
                            <div>
                                <div class="text-xl font-bold text-green-400">${dietPlan.macros.fats}g</div>
                                <div class="text-sm">Fats</div>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="space-y-4">
                    <div class="bg-white bg-opacity-10 rounded-lg p-4">
                        <h4 class="font-bold text-lg mb-2"><i class="fas fa-utensils mr-2"></i>Profession-Specific Tips</h4>
                        <ul class="space-y-2 text-sm">
                            ${dietPlan.professionTips.map(tip => `<li><i class="fas fa-check-circle text-green-400 mr-2"></i>${tip}</li>`).join('')}
                        </ul>
                    </div>
                    
                    <div class="bg-white bg-opacity-10 rounded-lg p-4">
                        <h4 class="font-bold text-lg mb-2"><i class="fas fa-star mr-2"></i>Goal-Specific Foods</h4>
                        <ul class="space-y-2 text-sm">
                            ${dietPlan.goalFoods.map(food => `<li><i class="fas fa-apple-alt text-green-400 mr-2"></i>${food}</li>`).join('')}
                        </ul>
                    </div>
                </div>
            `;
        }

        function generateDietPlan(profile) {
            // Calculate BMR using Mifflin-St Jeor Equation
            let bmr;
            if (profile.gender === 'male') {
                bmr = 88.362 + (13.397 * profile.weight) + (4.799 * profile.height) - (5.677 * profile.age);
            } else {
                bmr = 447.593 + (9.247 * profile.weight) + (3.098 * profile.height) - (4.330 * profile.age);
            }

            // Activity multiplier
            const activityMultipliers = {
                'sedentary': 1.2,
                'light': 1.375,
                'moderate': 1.55,
                'active': 1.725,
                'very-active': 1.9
            };

            let tdee = bmr * activityMultipliers[profile.activityLevel];

            // Adjust for goals
            if (profile.goal === 'weight-loss') {
                tdee -= 500; // 500 calorie deficit
            } else if (profile.goal === 'muscle-gain') {
                tdee += 300; // 300 calorie surplus
            }

            const calories = Math.round(tdee);

            // Calculate macros based on goal
            let proteinRatio, carbRatio, fatRatio;
            
            switch (profile.goal) {
                case 'weight-loss':
                    proteinRatio = 0.35; carbRatio = 0.35; fatRatio = 0.30;
                    break;
                case 'muscle-gain':
                    proteinRatio = 0.30; carbRatio = 0.45; fatRatio = 0.25;
                    break;
                case 'endurance':
                    proteinRatio = 0.20; carbRatio = 0.60; fatRatio = 0.20;
                    break;
                default:
                    proteinRatio = 0.25; carbRatio = 0.50; fatRatio = 0.25;
            }

            const macros = {
                protein: Math.round((calories * proteinRatio) / 4),
                carbs: Math.round((calories * carbRatio) / 4),
                fats: Math.round((calories * fatRatio) / 9)
            };

            // Profession-specific tips
            const professionTips = {
                'desk-job': [
                    'Pack healthy snacks to avoid vending machine temptations',
                    'Stay hydrated - aim for 8-10 glasses of water daily',
                    'Include anti-inflammatory foods like berries and leafy greens'
                ],
                'manual-labor': [
                    'Eat protein-rich meals for muscle recovery',
                    'Include complex carbs for sustained energy throughout the day',
                    'Consider electrolyte replacement during hot weather'
                ],
                'healthcare': [
                    'Prepare grab-and-go meals for busy shifts',
                    'Include stress-reducing foods like dark chocolate and nuts',
                    'Focus on immune-boosting foods like citrus and garlic'
                ],
                'student': [
                    'Brain-boosting foods like blueberries and fatty fish',
                    'Avoid energy crashes with balanced meals',
                    'Budget-friendly protein sources like eggs and legumes'
                ],
                'athlete': [
                    'Time carbohydrate intake around training sessions',
                    'Prioritize post-workout protein within 30 minutes',
                    'Include antioxidant-rich foods for recovery'
                ]
            };

            // Goal-specific foods
            const goalFoods = {
                'weight-loss': [
                    'Lean proteins: chicken breast, fish, tofu',
                    'High-fiber vegetables: broccoli, spinach, Brussels sprouts',
                    'Healthy fats: avocado, nuts, olive oil (in moderation)'
                ],
                'muscle-gain': [
                    'Complete proteins: eggs, milk, quinoa',
                    'Calorie-dense nuts and seeds',
                    'Complex carbs: oats, sweet potatoes, brown rice'
                ],
                'endurance': [
                    'Energy-rich carbs: bananas, dates, whole grain pasta',
                    'Electrolyte foods: coconut water, leafy greens',
                    'Anti-inflammatory foods: tart cherries, turmeric'
                ],
                'general-health': [
                    'Colorful vegetables and fruits for antioxidants',
                    'Whole grains for fiber and B vitamins',
                    'Lean proteins for muscle maintenance'
                ]
            };

            return {
                calories,
                calorieNote: profile.goal === 'weight-loss' ? 'Calorie deficit for weight loss' : 
                           profile.goal === 'muscle-gain' ? 'Calorie surplus for muscle gain' : 
                           'Maintenance calories for your goals',
                macros,
                professionTips: professionTips[profile.profession] || professionTips['desk-job'],
                goalFoods: goalFoods[profile.goal] || goalFoods['general-health']
            };
        }

        function displayWorkoutPlan() {
            const workoutPlan = generateWorkoutPlan(userProfile);
            const container = document.getElementById('workout-plan');
            
            container.innerHTML = `
                <div class="space-y-6">
                    <div class="bg-white bg-opacity-10 rounded-lg p-4">
                        <h4 class="font-bold text-lg mb-4"><i class="fas fa-calendar-alt mr-2"></i>Your Weekly Schedule</h4>
                        <div class="grid gap-3">
                            ${workoutPlan.weeklySchedule.map((day, index) => `
                                <div class="flex justify-between items-center p-3 bg-white bg-opacity-10 rounded">
                                    <span class="font-semibold">${['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'][index]}</span>
                                    <span class="text-sm">${day}</span>
                                </div>
                            `).join('')}
                        </div>
                    </div>
                    
                    <div class="grid md:grid-cols-2 gap-4">
                        <div class="bg-white bg-opacity-10 rounded-lg p-4">
                            <h4 class="font-bold text-lg mb-3"><i class="fas fa-dumbbell mr-2"></i>Sample Workout</h4>
                            <ul class="space-y-2 text-sm">
                                ${workoutPlan.sampleWorkout.map(exercise => `
                                    <li class="flex justify-between">
                                        <span>${exercise.name}</span>
                                        <span class="text-gray-300">${exercise.sets}</span>
                                    </li>
                                `).join('')}
                            </ul>
                        </div>
                        
                        <div class="bg-white bg-opacity-10 rounded-lg p-4">
                            <h4 class="font-bold text-lg mb-3"><i class="fas fa-lightbulb mr-2"></i>Training Tips</h4>
                            <ul class="space-y-2 text-sm">
                                ${workoutPlan.tips.map(tip => `<li><i class="fas fa-check text-green-400 mr-2"></i>${tip}</li>`).join('')}
                            </ul>
                        </div>
                    </div>
                </div>
            `;
        }

        function generateWorkoutPlan(profile) {
            const isGymUser = profile.gymAccess === 'yes';
            const workoutDays = parseInt(profile.workoutDays.split('-')[0]);
            
            // Generate weekly schedule
            const scheduleOptions = {
                gym: {
                    3: ['Upper Body', 'Lower Body', 'Full Body', 'Rest', 'Rest', 'Rest', 'Rest'],
                    4: ['Upper Body', 'Lower Body', 'Push', 'Pull', 'Rest', 'Rest', 'Rest'],
                    5: ['Chest/Triceps', 'Back/Biceps', 'Legs', 'Shoulders', 'Full Body', 'Rest', 'Rest'],
                    6: ['Push', 'Pull', 'Legs', 'Push', 'Pull', 'Legs', 'Rest']
                },
                home: {
                    3: ['Full Body', 'Cardio', 'Full Body', 'Rest', 'Rest', 'Rest', 'Rest'],
                    4: ['Upper Body', 'Lower Body', 'Cardio', 'Full Body', 'Rest', 'Rest', 'Rest'],
                    5: ['Upper Body', 'Lower Body', 'Cardio', 'Full Body', 'Core/Flexibility', 'Rest', 'Rest'],
                    6: ['Upper Body', 'Lower Body', 'Cardio', 'Upper Body', 'Lower Body', 'Active Recovery', 'Rest']
                }
            };

            const scheduleType = isGymUser ? 'gym' : 'home';
            const weeklySchedule = scheduleOptions[scheduleType][Math.min(workoutDays, 6)] || scheduleOptions[scheduleType][3];

            // Generate sample workout based on goal and gym access
            let sampleWorkout;
            if (isGymUser) {
                sampleWorkout = getGymWorkout(profile.goal);
            } else {
                sampleWorkout = getHomeWorkout(profile.goal);
            }

            // Generate tips based on profile
            const tips = getWorkoutTips(profile);

            return {
                weeklySchedule,
                sampleWorkout,
                tips
            };
        }

        function getGymWorkout(goal) {
            const workouts = {
                'weight-loss': [
                    { name: 'Treadmill/Elliptical', sets: '20-30 min' },
                    { name: 'Squats', sets: '3x12-15' },
                    { name: 'Bench Press', sets: '3x10-12' },
                    { name: 'Rowing Machine', sets: '3x10-12' },
                    { name: 'Plank', sets: '3x30-60s' },
                    { name: 'Burpees', sets: '3x8-10' }
                ],
                'muscle-gain': [
                    { name: 'Squats', sets: '4x6-8' },
                    { name: 'Deadlifts', sets: '4x6-8' },
                    { name: 'Bench Press', sets: '4x6-8' },
                    { name: 'Pull-ups', sets: '3x8-12' },
                    { name: 'Overhead Press', sets: '3x8-10' },
                    { name: 'Barbell Rows', sets: '3x8-10' }
                ],
                'strength': [
                    { name: 'Deadlifts', sets: '5x5' },
                    { name: 'Squats', sets: '5x5' },
                    { name: 'Bench Press', sets: '5x5' },
                    { name: 'Overhead Press', sets: '5x5' },
                    { name: 'Barbell Rows', sets: '5x5' }
                ],
                'endurance': [
                    { name: 'Treadmill Intervals', sets: '30 min' },
                    { name: 'Bike Sprints', sets: '10x30s' },
                    { name: 'Circuit Training', sets: '3 rounds' },
                    { name: 'Battle Ropes', sets: '3x45s' },
                    { name: 'Box Jumps', sets: '3x15' }
                ]
            };

            return workouts[goal] || workouts['weight-loss'];
        }

        function getHomeWorkout(goal) {
            const workouts = {
                'weight-loss': [
                    { name: 'Jumping Jacks', sets: '3x30' },
                    { name: 'Bodyweight Squats', sets: '3x15' },
                    { name: 'Push-ups', sets: '3x10-15' },
                    { name: 'Mountain Climbers', sets: '3x20' },
                    { name: 'Plank', sets: '3x30-60s' },
                    { name: 'Burpees', sets: '3x8' }
                ],
                'muscle-gain': [
                    { name: 'Push-up Variations', sets: '4x8-12' },
                    { name: 'Single-leg Squats', sets: '3x8 each' },
                    { name: 'Pike Push-ups', sets: '3x8-10' },
                    { name: 'Lunge Variations', sets: '3x12 each' },
                    { name: 'Tricep Dips', sets: '3x10-15' },
                    { name: 'Wall Sits', sets: '3x45-60s' }
                ],
                'endurance': [
                    { name: 'High Knees', sets: '3x45s' },
                    { name: 'Jump Squats', sets: '3x15' },
                    { name: 'Burpees', sets: '3x10' },
                    { name: 'Running in Place', sets: '5 min' },
                    { name: 'Jump Rope (imaginary)', sets: '3x60s' }
                ]
            };

            return workouts[goal] || workouts['weight-loss'];
        }

        function getWorkoutTips(profile) {
            const baseTips = [
                'Always warm up for 5-10 minutes before exercising',
                'Focus on proper form over heavy weights',
                'Stay hydrated throughout your workout',
                'Get adequate rest between workout sessions'
            ];

            const goalTips = {
                'weight-loss': ['Combine cardio with strength training', 'Track your calorie burn'],
                'muscle-gain': ['Progressive overload is key', 'Eat protein within 30 minutes post-workout'],
                'strength': ['Focus on compound movements', 'Allow 48-72 hours rest between sessions'],
                'endurance': ['Gradually increase workout duration', 'Include both steady-state and interval training']
            };

            const professionTips = {
                'desk-job': ['Take movement breaks every hour', 'Focus on posture-correcting exercises'],
                'manual-labor': ['Include flexibility and mobility work', 'Don\'t overtrain - you\'re already active'],
                'healthcare': ['Schedule workouts during off-shifts', 'Stress-relief exercises are important']
            };

            return [
                ...baseTips,
                ...(goalTips[profile.goal] || []),
                ...(professionTips[profile.profession] || [])
            ].slice(0, 6);
        }

        function restartAssessment() {
            currentStep = 1;
            userProfile = {};
            
            // Reset form
            document.getElementById('assessment-form').querySelectorAll('input, select').forEach(element => {
                element.value = '';
                if (element.type === 'radio') {
                    element.checked = false;
                }
            });
            
            // Show welcome screen
            document.getElementById('results-screen').classList.add('hidden');
            document.getElementById('welcome-screen').classList.remove('hidden');
        }

        function shareResults() {
            const shareData = {
                title: 'My FitQuest Results',
                text: `I just completed FitQuest fitness assessment! Energy Level: ${document.getElementById('energy-level').textContent}, Fitness Age: ${document.getElementById('fitness-age').textContent}`,
                url: window.location.href
            };

            if (navigator.share) {
                navigator.share(shareData);
            } else {
                // Fallback for browsers that don't support Web Share API
                navigator.clipboard.writeText(`${shareData.text}\n${shareData.url}`).then(() => {
                    alert('Results copied to clipboard!');
                }).catch(() => {
                    alert('Unable to share results. Please copy the URL manually.');
                });
            }
        }

        // Initialize the app
        document.addEventListener('DOMContentLoaded', function() {
            // Add any initialization code here
        });
    </script>
</body>
</html>
