<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IELTS Vocabulary Builder</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');

        body {
            font-family: 'Inter', sans-serif;
            overflow: hidden; /* Prevent scrollbars due to stars */
        }

        /* Animated Background - Stars */
        .stars {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(to bottom, #2a0a5e, #000000); /* Dark blue to black gradient */
            overflow: hidden;
            z-index: -1;
        }

        .star {
            position: absolute;
            background-color: white;
            border-radius: 50%;
            opacity: 0;
            animation: twinkle linear infinite;
        }

        /* Star animation */
        @keyframes twinkle {
            0% { opacity: 0; transform: scale(0.5); }
            50% { opacity: 1; transform: scale(1); }
            100% { opacity: 0; transform: scale(0.5); }
        }

        /* Flashcard specific styles */
        .flashcard-container {
            perspective: 1000px; /* For 3D flip effect */
            width: 100%;
            max-width: 600px;
            height: 350px; /* Fixed height for flashcards */
            margin: 2rem auto;
            position: relative;
        }

        .flashcard {
            width: 100%;
            height: 100%;
            position: absolute;
            transform-style: preserve-3d;
            transition: transform 0.6s;
            border-radius: 1rem;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.5); /* Stronger shadow */
            background-color: rgba(42, 10, 94, 0.8); /* Dark purple, matching background */
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 1.5rem;
            color: #fff; /* White text for dark background */
        }

        .flashcard.flipped {
            transform: rotateY(180deg);
        }

        .flashcard-front, .flashcard-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 1.5rem;
            border-radius: 1rem;
            box-sizing: border-box;
        }

        .flashcard-back {
            transform: rotateY(180deg);
            background-color: rgba(60, 20, 120, 0.85); /* Slightly lighter dark purple for back */
        }

        .flashcard-word {
            font-size: 2.5rem;
            font-weight: 700;
            color: #a78bfa; /* Lighter purple for word */
            margin-bottom: 1rem;
        }

        .flashcard-definition {
            font-size: 1.25rem;
            font-weight: 600;
            color: #e0e7ff; /* Light blue-white for definition */
            margin-bottom: 1rem;
        }

        .flashcard-examples {
            font-size: 1rem;
            color: #c7d2fe; /* Even lighter blue-white for examples */
            text-align: left;
            width: 100%;
            max-height: 120px; /* Limit height for examples */
            overflow-y: auto; /* Enable scrolling for long examples */
        }

        .flashcard-examples ul {
            list-style-type: disc;
            margin-left: 1.5rem;
        }
        .flashcard-examples li {
            margin-bottom: 0.5rem;
        }

        /* Progress Bar */
        .progress-bar-container {
            width: 80%;
            max-width: 600px;
            height: 15px;
            background-color: #e0e0e0;
            border-radius: 9999px;
            margin: 1rem auto;
            overflow: hidden;
            box-shadow: inset 0 1px 3px rgba(0,0,0,0.2);
        }

        .progress-bar {
            height: 100%;
            width: 0%;
            background-color: #a78bfa; /* Lighter purple for progress */
            border-radius: 9999px;
            transition: width 0.5s ease-in-out;
        }

        /* Quiz Section */
        .quiz-container {
            background-color: rgba(42, 10, 94, 0.8); /* Dark purple, matching background */
            border-radius: 1rem;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.5);
            padding: 2rem;
            margin: 2rem auto;
            max-width: 700px;
            color: #fff; /* White text for dark background */
        }

        .quiz-question {
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 1.5rem;
            color: #a78bfa; /* Lighter purple for question */
        }

        .quiz-options button {
            display: block;
            width: 100%;
            padding: 0.75rem 1.25rem;
            margin-bottom: 0.75rem;
            background-color: rgba(255, 255, 255, 0.1); /* Slightly translucent white */
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 0.5rem;
            text-align: left;
            font-size: 1.1rem;
            cursor: pointer;
            transition: background-color 0.3s, border-color 0.3s;
            color: #e0e7ff; /* Light blue-white text */
        }

        .quiz-options button:hover {
            background-color: rgba(255, 255, 255, 0.2);
        }

        .quiz-options button.correct {
            background-color: #28a745; /* Green */
            border-color: #28a745;
            font-weight: 600;
            color: white;
        }

        .quiz-options button.incorrect {
            background-color: #dc3545; /* Red */
            border-color: #dc3545;
            font-weight: 600;
            color: white;
        }

        .quiz-feedback {
            margin-top: 1rem;
            font-size: 1.1rem;
            font-weight: 600;
            text-align: center;
        }

        .quiz-score {
            font-size: 1.25rem;
            font-weight: 700;
            margin-top: 1.5rem;
            text-align: center;
            color: #a78bfa;
        }

        /* Custom Modal */
        .custom-modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            visibility: hidden;
            opacity: 0;
            transition: visibility 0s, opacity 0.3s;
        }

        .custom-modal-overlay.show {
            visibility: visible;
            opacity: 1;
        }

        .custom-modal-content {
            background-color: rgba(42, 10, 94, 0.9); /* Dark purple, matching background */
            padding: 2rem;
            border-radius: 1rem;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
            max-width: 400px;
            text-align: center;
            color: #fff;
        }

        .custom-modal-content h3 {
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 1rem;
            color: #a78bfa;
        }

        .custom-modal-content p {
            margin-bottom: 1.5rem;
        }

        .custom-modal-content button {
            padding: 0.75rem 1.5rem;
            background-color: #6a0dad;
            color: white;
            border-radius: 0.5rem;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .custom-modal-content button:hover {
            background-color: #5a0c9c;
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex flex-col items-center justify-center relative">

    <!-- Animated Stars Background -->
    <div class="stars" id="star-background"></div>

    <div class="container mx-auto p-4 md:p-8 relative z-10 w-full">
        <h1 class="text-4xl md:text-5xl font-extrabold text-white text-center mb-6 drop-shadow-lg">
            IELTS Vocabulary Builder
        </h1>

        <!-- Category Selection -->
        <div class="flex flex-col md:flex-row items-center justify-center gap-4 mb-8">
            <label for="category-select" class="text-white text-lg font-semibold">Select Letter:</label>
            <select id="category-select" class="p-3 rounded-lg shadow-md bg-white text-gray-800 border border-gray-300 focus:outline-none focus:ring-2 focus:ring-purple-500">
                <!-- Options will be dynamically loaded here -->
            </select>
            <button id="start-quiz-btn" class="px-6 py-3 bg-purple-700 text-white font-bold rounded-lg shadow-lg hover:bg-purple-800 transition duration-300 ease-in-out transform hover:scale-105">
                Start Quiz
            </button>
            <button id="flashcard-mode-btn" class="px-6 py-3 bg-blue-600 text-white font-bold rounded-lg shadow-lg hover:bg-blue-700 transition duration-300 ease-in-out transform hover:scale-105 hidden">
                Flashcard Mode
            </button>
        </div>

        <!-- Flashcard Section -->
        <div id="flashcard-section" class="flashcard-container">
            <div id="flashcard" class="flashcard cursor-pointer">
                <div class="flashcard-front">
                    <p class="flashcard-word" id="flashcard-word-front">Select a Letter to Begin!</p>
                </div>
                <div class="flashcard-back">
                    <p class="flashcard-word" id="flashcard-word-back"></p>
                    <p class="flashcard-definition" id="flashcard-definition"></p>
                    <div class="flashcard-examples" id="flashcard-examples"></div>
                </div>
            </div>
        </div>

        <!-- Navigation Buttons -->
        <div id="navigation-buttons" class="flex justify-center gap-4 mt-8">
            <button id="prev-btn" class="px-6 py-3 bg-green-600 text-white font-bold rounded-lg shadow-lg hover:bg-green-700 transition duration-300 ease-in-out transform hover:scale-105">
                Previous
            </button>
            <button id="next-btn" class="px-6 py-3 bg-green-600 text-white font-bold rounded-lg shadow-lg hover:bg-green-700 transition duration-300 ease-in-out transform hover:scale-105">
                Next
            </button>
        </div>

        <!-- Progress Bar -->
        <div class="progress-bar-container">
            <div id="progress-bar" class="progress-bar"></div>
        </div>
        <p id="progress-text" class="text-white text-center mt-2 text-sm">0/0 words</p>

        <!-- Quiz Section -->
        <div id="quiz-section" class="quiz-container hidden">
            <p class="quiz-question" id="quiz-question"></p>
            <div class="quiz-options" id="quiz-options">
                <!-- Options will be dynamically loaded here -->
            </div>
            <p class="quiz-feedback" id="quiz-feedback"></p>
            <button id="next-quiz-btn" class="px-6 py-3 bg-purple-700 text-white font-bold rounded-lg shadow-lg hover:bg-purple-800 transition duration-300 ease-in-out transform hover:scale-105 mt-4 w-full">
                Next Question
            </button>
            <p class="quiz-score" id="quiz-score">Score: 0 / 0</p>
        </div>
    </div>

    <!-- Custom Modal for Messages -->
    <div id="custom-modal-overlay" class="custom-modal-overlay">
        <div class="custom-modal-content">
            <h3 id="modal-title"></h3>
            <p id="modal-message"></p>
            <button id="modal-close-btn">OK</button>
        </div>
    </div>

    <script>
        // Consolidated Vocabulary Data (Sorted Alphabetically)
        // This data is based on the previous extraction.
        // To get all 601 words, you would need to manually add them here.
        const allWords = [
            { word: "abroad", definition: "in a foreign country", examples: ["Thousands working abroad and sending money to relatives back home.", "Many students choose to study abroad."] },
            { word: "absorb", definition: "to take in", examples: ["Scientists look at how much drug is absorbed into the bloodstream.", "The body absorbs some drugs very quickly."] },
            { word: "abstract", definition: "not concrete, related to ideas or feelings", examples: ["Miming can be abstract, literal, or a combination of the two. Abstract miming usually has no plot or central character but simply expresses a feeling.", "Abstract art often focuses on shapes and colors rather than realistic depictions."] },
            { word: "accelerate", definition: "to gain speed", examples: ["An electric sports car that can travel 300 miles on a single charge and accelerate from 0 to 60 mph in 3.7 seconds.", "The car accelerated quickly on the highway."] },
            { word: "accessible", definition: "reachable, easy to get", examples: ["This has made it a luxury accessible to fewer people.", "Machu Picchu is accessible by plane, train, or hiking."] },
            { word: "accommodations", definition: "a place to stay such as a hotel", examples: ["Ecotourists seek out accommodations that follow environmentally friendly practices.", "The accommodations on our trip were very comfortable."] },
            { word: "accompany", definition: "to go with, happen at the same time", examples: ["Hula was accompanied by great ritual.", "Guitars often accompany modern hula dances."] },
            { word: "accumulate", definition: "to gradually increase over time", examples: ["Tamarisk accumulates salt in special glands.", "Dust tends to accumulate on surfaces if not cleaned regularly."] },
            { word: "acknowledge", definition: "to admit, accept as true", examples: ["Authorities have begun to acknowledge the problem.", "It's crucial to acknowledge the importance of leisure-time activities."] },
            { word: "acquire", definition: "to learn something or get something", examples: ["Acquiring new skills that they can take home with them.", "It is fun to acquire new skills while on vacation."] },
            { word: "adapt", definition: "to change to fit a situation or environment", examples: ["Diverse plant forms have not only adapted to the harsh conditions but actually thrive there.", "One way that plants adapt to the dry desert is by developing deep root systems."] },
            { word: "administer", definition: "to give medicine or medical treatment", examples: ["Some patients go untreated not because lifesaving drugs are unavailable but because there are not enough nurses to administer them.", "The nurse administered the medication."] },
            { word: "adventurous", definition: "daring, willing to try new or dangerous activities", examples: ["A popular route for the more adventurous is to hike along the Inca Trail.", "Adventurous people enjoy hiking in the Andes Mountains."] },
            { word: "aerobic", definition: "relating to energetic exercise", examples: ["The disease-fighting, weight-controlling benefits of physical exercise, especially aerobic exercise, have long been known.", "Aerobic exercise can also improve short-term memory in the elderly."] },
            { word: "afloat", definition: "having enough money to pay what you owe", examples: ["Projects the cash flow that will help the business stay afloat.", "The company struggled to stay afloat during the recession."] },
            { word: "agricultural", definition: "related to farming", examples: ["Along with the rise of agricultural societies came the development of property ownership.", "Wheat was one of the first agricultural products."] },
            { word: "alleviate", definition: "to lessen, ease", examples: ["Drug trials may focus on treating a disease, preventing a disease from occurring or recurring, or enhancing the quality of life for people living with incurable, chronic conditions.", "The medication will alleviate your pain."] },
            { word: "altar", definition: "a table or similar structure for religious ceremonies", examples: ["It was most likely originally performed in front of an altar in honor of gods.", "Offerings were placed on the altar."] },
            { word: "ancient", definition: "very old, of the distant past", examples: ["The ancient Romans were the first to enjoy the circus.", "The pyramids are ancient structures."] },
            { word: "anticipate", definition: "to expect, be ready for something to happen", examples: ["Good footwork and body positioning will help the athlete gain viewing time in this intense environment, improving the opportunity to anticipate what will happen next.", "We anticipate a busy day at the office tomorrow."] },
            { word: "appeal", definition: "to be of interest", examples: ["Slow, short-ranged electric vehicles with a high initial cost have thus far appealed to a limited audience.", "A car that is inexpensive to buy and easy to maintain would appeal to many people."] },
            { word: "aquatic", definition: "living in the water", examples: ["Many life forms—both terrestrial and aquatic—are becoming endangered as forests vanish.", "Fish and other aquatic wildlife are affected by water pollution."] },
            { word: "archeologist", definition: "a person who studies ancient cultures", examples: ["Rediscovered by archeologist Hiram Bingham in 1911.", "An archeologist is interested in ancient cultures."] },
            { word: "architecture", definition: "the style of a building", examples: ["The Paris Metro is known for its beauty. The stations and entrances are examples of art nouveau architecture.", "The architecture of the stations is an important part of subway system design."] },
            { word: "array", definition: "a large number, a collection", examples: ["The logging industry supplies the raw materials for an array of products.", "There is an array of beautiful flowers in the garden."] },
            { word: "ascertain", definition: "to determine; find out", examples: ["Phase I clinical trials test a drug in small groups of healthy volunteers to ascertain its safety.", "We need to ascertain the facts before making a decision."] },
            { word: "aspect", definition: "a part or feature", examples: ["How birds manage to unerringly travel between distant locations is one aspect that has fascinated observers for centuries.", "Punctuality is an important aspect of professionalism."] },
            { word: "athlete", definition: "a person who plays sports", examples: ["The fast-moving athlete must detect all kinds of motion from every angle.", "An athlete's performance depends on training visual abilities."] },
            { word: "atmosphere", definition: "the feeling of a place", examples: ["Sounds, lighting, and other special effects are included to help create the desired atmosphere.", "The bright lighting in the theater created a happy atmosphere."] },
            { word: "attribute", definition: "to give credit for or see as the origin of something", examples: ["Ancient civilizations attributed the origins of writing to the gods.", "The invention of the lightbulb is attributed to Thomas Edison."] },
            { word: "authority", definition: "person with power or special knowledge", examples: ["Authorities have begun to acknowledge the problem.", "According to an authoritative source, spending time in nature improves our health."] },
            { word: "avoid", definition: "to prevent from happening; stay away from", examples: ["Define ecotourism as travel that aims not only to avoid harming the environment.", "We can avoid damaging the environment if we are careful."] },
            { word: "band", definition: "a small group", examples: ["Public entertainment was reduced to small bands of traveling performers.", "A band of musicians played at the festival."] },
            { word: "barrier", definition: "something that blocks or separates", examples: ["Ecotourists might choose to join a bicycling or walking tour rather than a bus tour that adds to air pollution and allows tourists to see the local area only through a barrier of glass windows.", "The river forms a natural barrier between the two countries."] },
            { word: "benefit", definition: "the use, advantage of", examples: ["Performed graceful island dances for the benefit of tourists.", "A benefit of hula dancing is that it attracts people to Hawaii."] },
            { word: "blur", definition: "something not seen clearly", examples: ["We tolerate this large outlying field of blur.", "The distant lights were just a blur in the fog."] },
            { word: "boundary", definition: "an edge, border", examples: ["A volleyball player must pay attention to the court boundaries.", "The fence marks the boundary of our property."] },
            { word: "breed", definition: "to reproduce", examples: ["Migration is the regular movement of animals between their breeding grounds.", "Birds travel to higher latitudes to breed during the warm season."] },
            { word: "breeze", definition: "light wind", examples: ["Enjoying, for example, the warm breeze of the Caribbean islands.", "A gentle breeze rustled the leaves."] },
            { word: "broad", definition: "wide or large", examples: ["With the broad range of possibilities available.", "The company has a broad range of products."] },
            { word: "budget", definition: "a plan for spending money", examples: ["There are options to suit all tastes and budgets.", "We need to stick to our budget for the trip."] },
            { word: "bulk", definition: "the largest part", examples: ["Leaving the bulk of health care up to assistant nurses.", "The bulk of the work was completed yesterday."] },
            { word: "carve", definition: "to cut and shape hard material", examples: ["Oracle bone script was carved on animal bones and shells.", "The artist carved a beautiful sculpture from wood."] },
            { word: "cast", definition: "to throw light on something", examples: ["Flames cast limited light.", "The lamp cast a warm glow on the room."] },
            { word: "capacity", definition: "total amount available", examples: ["Exercise can repair some of the loss in brain capacity associated with aging.", "The hall has a seating capacity of 500 people."] },
            { word: "category", definition: "a group of things that have something in common", examples: ["The types of vacations that fit into the category of ecotourism vary widely.", "This book falls into the category of science fiction."] },
            { word: "celebration", definition: "a social event to mark a special day or occasion", examples: ["Hula was performed at celebrations held in his honor.", "Hula dances are often performed at cultural celebrations."] },
            { word: "centerpiece", definition: "the main or most important feature", examples: ["Many big cities have an underground rail system as their centerpiece.", "The large fountain was the centerpiece of the park."] },
            { word: "century", definition: "a period of 100 years", examples: ["Although the modern circus has been around for a few centuries.", "The 20th century saw many technological advancements."] },
            { word: "ceremonial", definition: "related to traditional or formal practices", examples: ["The Incas built a ceremonial city on the site.", "The building is open only to groups with guides, and we'll be visiting it on our ceremonial area."] },
            { word: "charge", definition: "the amount of power a battery can store", examples: ["Could travel eighty-five miles on a single charge.", "This car can travel about 100 miles on one battery charge."] },
            { word: "characteristic", definition: "a feature, quality", examples: ["Certain characteristics make a small business more likely to succeed.", "Punctuality is a characteristic of a good employee."] },
            { word: "chunk", definition: "a large piece", examples: ["Vacations are the most obvious chunk of leisure time.", "He ate a large chunk of bread."] },
            { word: "chronic", definition: "long-lasting", examples: ["Enhancing the quality of life for people living with incurable, chronic conditions.", "He suffers from chronic back pain."] },
            { word: "civilization", definition: "human society, its organization and culture", examples: ["Ancient civilizations attributed the origins of writing to the gods.", "The Mayan civilization was highly advanced."] },
            { word: "classify", definition: "to divide into groups by type", examples: ["One is a three-wheeler that is classified as a motorcycle.", "We need to classify these documents by date."] },
            { word: "clog", definition: "to fill so much as to make movement difficult", examples: ["Horses and pedestrians had so clogged the streets of London.", "Leaves clogged the drainpipe."] },
            { word: "cognition", definition: "the use of mental processes", examples: ["Exercise can reduce the age-associated loss of brain tissue that decreases cognition in the elderly and in those who have disorders such as Alzheimer's disease.", "The test measures various aspects of human cognition."] },
            { word: "combat", definition: "to fight against", examples: ["Phase II clinical trials test several hundred volunteers to determine how effectively the drug combats the disease.", "The new drug helps combat the infection."] },
            { word: "colorful", definition: "interesting and unusual", examples: ["The colorful villages of Spain.", "The market was full of colorful fruits and vegetables."] },
            { word: "complicate", definition: "to cause to be more difficult", examples: ["Tracking fast objects is often complicated by the need for the athlete's body to move.", "The need to pay attention to many things at once complicates the game for an athlete."] },
            { word: "commuter", definition: "a person who travels regularly between home and work", examples: ["Other American auto manufacturers are marketing electric cars as they do in Europe, as commuter cars.", "Commuters are worried about the increase of traffic on the highways."] },
            { word: "compete", definition: "to do as well as or better than others", examples: ["It is difficult for a small business to compete with the array of products or services a large business can offer.", "Small businesses often struggle to compete with large corporations."] },
            { word: "complex", definition: "not simple", examples: ["Because the underlying causes are complex and vary in different regions.", "The reasons for the worldwide nursing shortage are complex."] },
            { word: "concentration", definition: "a large amount of something in the same place", examples: ["Researchers found increased concentrations of BDNF in the hippocampus.", "High concentrations of pollutants were found in the water."] },
            { word: "concept", definition: "idea", examples: ["The concept of ecotourism has been gaining publicity.", "Ecotourism is a concept that is growing in popularity."] },
            { word: "construct", definition: "to build", examples: ["All constructed from heavy blocks of granite.", "How were the ancient people able to construct such spectacular buildings?"] },
            { word: "consume", definition: "to use", examples: ["The reserves of irreplaceable oil are consumed.", "Electric cars are attractive because they don't consume gasoline."] },
            { word: "content", definition: "subject matter", examples: ["The content of the article was quite interesting.", "The course content covers a wide range of topics."] },
            { word: "coordinate", definition: "to organize; make work together", examples: ["Head motion must coordinate with eye movement to assist in balance.", "An athlete must coordinate physical skill with sharp vision."] },
            { word: "costly", definition: "expensive", examples: ["Some companies offer learning vacations to exotic locales with expert professionals that are quite costly.", "Learning vacations can be costly, but usually they are not."] },
            { word: "counteract", definition: "to work against", examples: ["Exercise can counteract this effect.", "We need to find a way to counteract the negative effects of stress."] },
            { word: "creator", definition: "the first maker of something", examples: ["For the ancient Egyptians, their god Thoth was the creator of writing.", "The ancient Maya were the creators of temples."] },
            { word: "cripple", definition: "to cause serious damage; weaken", examples: ["A global nursing shortage threatens to cripple health care systems worldwide.", "The ability to administer health care where it is needed is crippled when there are not enough qualified nurses."] },
            { word: "crucial", definition: "very important", examples: ["Spending time in natural surroundings is especially crucial now.", "It is crucial to acknowledge the importance of leisure-time activities."] },
            { word: "cuisine", definition: "style of cooking", examples: ["Tour companies have developed trips that focus on the cuisine of different regions of the world.", "French cuisine is famous worldwide."] },
            { word: "culminate", definition: "to result in, end with", examples: ["The art of miming grew to include acrobatics, props, and costumes, culminating in the fine-tuned art form that people recognize today.", "The celebration culminated in a fireworks display."] },
            { word: "culprit", definition: "guilty party, origin of a problem", examples: ["These are among the biggest culprits in the tourism industry in terms of environmental pollution.", "The police are still looking for the culprit."] },
            { word: "culture", definition: "the growing of organic materials in a laboratory setting", examples: ["They test them in laboratory cultures.", "A clinic might use a culture from the patient to diagnose a disease."] },
            { word: "decade", definition: "period of ten years", examples: ["Underinvestment in nursing education dating back two or more decades.", "What are your professional goals for the next decade?"] },
            { word: "decline", definition: "to gradually go lower, become smaller", examples: ["The nurse-to-population ratio declines.", "There has been a decline of applicants for nursing programs."] },
            { word: "decorate", definition: "to make an object or place beautiful", examples: ["They are decorated with mosaics, sculptures, paintings, and innovative doors and walls.", "Sometimes they decorate the trains for the holidays."] },
            { word: "deed", definition: "an act, especially a good or bad one", examples: ["They used a system of symbols to record information about the deeds of their rulers.", "His heroic deeds were celebrated by the community."] },
            { word: "defense", definition: "protection", examples: ["Trees provide a natural defense against air pollution.", "The shade from trees provides a defense against the drying effects of the sun."] },
            { word: "deforestation", definition: "the removal of all trees from a large area", examples: ["Deforestation impacts rainfall patterns, leading to flooding as well as drought and forest fires.", "Deforestation contributes to the effects of both air and water pollution."] },
            { word: "delicate", definition: "easily hurt or broken", examples: ["Cruise ships also cause damage to coral reefs and other delicate ecosystems.", "The antique vase is very delicate."] },
            { word: "deliberately", definition: "intentionally, on purpose", examples: ["We have to rejuvenate ourselves in nature deliberately.", "We need to spend time in nature deliberately."] },
            { word: "dementia", definition: "the loss of intellectual functioning of the brain", examples: ["The hippocampus is an area of the brain involved in learning and memory and associated with dementia.", "Alzheimer's disease is a common cause of dementia."] },
            { word: "demonstrate", definition: "to show; model", examples: ["Athletes best demonstrate just how much we can use the entire range of our vision.", "Professional athletes demonstrate a high level of skills."] },
            { word: "depression", definition: "constant sadness", examples: ["Childhood obesity and depression are reaching epidemic levels.", "Mild depression may be a temporary response to the normal stresses of life, but ongoing depression could indicate a more serious condition."] },
            { word: "desirable", definition: "wanted; worth having", examples: ["Only a small fraction will produce the desired result.", "The most desirable type of drug is one that is effective and has no side effects."] },
            { word: "destination", definition: "the place somebody or something is going to", examples: ["Ecotourism might involve travel to a natural destination such as a national park.", "Our final destination is Paris."] },
            { word: "detect", definition: "to notice, become aware of", examples: ["The fast-moving athlete must detect all kinds of motion from every angle.", "It's important to detect problems early."] },
            { word: "deterioration", definition: "the situation of becoming worse", examples: ["Their brain scans showed less deterioration and more gray matter.", "The deterioration of the old building was evident."] },
            { word: "determine", definition: "to decide", examples: ["Tamarisk can naturally determine when to close stomata to inhibit evaporation.", "Scientists try to determine the meaning of ancient texts."] },
            { word: "develop", definition: "to grow and change", examples: ["It was during the Dark Ages that the circus began to develop into what we know today.", "The circus has developed in different ways over the years."] },
            { word: "diagnose", definition: "to identify an illness", examples: ["People who have received a diagnosis of major depression typically have lower concentrations of BDNF.", "It is not always easy to diagnose a disease."] },
            { word: "dilute", definition: "to make weaker by mixing with water", examples: ["P. euphratica dilutes the normally toxic salt by increasing the number and volume of its cells.", "You need to dilute the concentrate with water before drinking it."] },
            { word: "disaster", definition: "a terrible event", examples: ["Flames can lead to disaster.", "The earthquake was a natural disaster."] },
            { word: "discourage", definition: "to try to stop or prevent something", examples: ["Although hula did not completely disappear after contact, it was discouraged.", "His parents discouraged him from pursuing a career in art."] },
            { word: "disorder", definition: "a disease or illness", examples: ["Exercise can reduce the age-associated loss of brain tissue that decreases cognition in the elderly and in those who have disorders such as Alzheimer's disease.", "Depression is a serious disorder."] },
            { word: "display", definition: "to show or exhibit", examples: ["Schenectady and Troy Railroad trains displayed a whale oil lamp.", "The new, more efficient headlamps for use on trains were on display."] },
            { word: "disruptive", definition: "stopping the usual course of activity", examples: ["Although the method was disruptive, it worked.", "The process of building a subway can be disruptive."] },
            { word: "distracting", definition: "taking attention away from something", examples: ["The office worker might notice the tiny distracting insect.", "Loud music can be very distracting when you're trying to study."] },
            { word: "diurnal", definition: "active during the day", examples: ["Diurnal migrants, those migrating during the day, take their cues from the location of the sun.", "Most humans are diurnal creatures."] },
            { word: "diverse", definition: "varied, of many kinds", examples: ["Transitional areas support diverse plant forms.", "The diverse ways that plants adapt to desert conditions makes a fascinating study."] },
            { word: "draw", definition: "to attract, pull", examples: ["The mysteries of the ancient culture draw thousands of tourists.", "The interesting birds and flowers draw many people to the mountains."] },
            { word: "drawback", definition: "a problem; disadvantage", examples: ["This headlamp had two major drawbacks.", "One drawback of living in the city is the noise."] },
            { word: "dump", definition: "to get rid of garbage and trash", examples: ["Wastewater is often dumped into the sea.", "Please don't dump your trash here."] },
            { word: "economical", definition: "inexpensive", examples: ["Another advantage of these types of vacations is that they can be more economical than traditional vacations.", "Taking a learning vacation can be an economical way to travel."] },
            { word: "edge", definition: "an advantage", examples: ["Maintaining a competitive edge.", "Having a unique product gives them an an edge over competitors."] },
            { word: "effectively", definition: "well, successfully", examples: ["His tragicomic 'little tramp' character, who so effectively portrays human frailty.", "A skilled mime can effectively perform a variety of illusions."] },
            { word: "efficient", definition: "able to work without waste", examples: ["George C. Pyle developed an efficient electric headlamp.", "Efficient headlamps made safe travel at night possible."] },
            { word: "elaborate", definition: "having a lot of detail and decoration", examples: ["The dancers wear elaborate costumes.", "The wedding dress was very elaborate."] },
            { word: "emerge", definition: "to appear, develop", examples: ["Marcel Marceau emerged as what many consider the master of modern mime.", "New ideas are constantly emerging in the field of technology."] },
            { word: "embrace", definition: "to accept something enthusiastically", examples: ["Taxi drivers in Tokyo have embraced electric vehicles.", "The hope is that North Americans will embrace the new technology."] },
            { word: "emotion", definition: "a strong feeling such as anger or love", examples: ["Time spent in the city results in a decreased ability to concentrate and to control emotions and impulses.", "The stress of city life can make emotions difficult to control."] },
            { word: "encompass", definition: "to include", examples: ["Mesoamerica, a region that encompasses parts of Mexico and Central America.", "The course will encompass a wide range of topics."] },
            { word: "endeavor", definition: "activity with a specific purpose, effort", examples: ["It has become common for adults, too, to spend their vacation time in educational endeavors.", "Building this house was a huge endeavor."] },
            { word: "endure", definition: "to live under difficult conditions", examples: ["Rock ptarmigan spend the winter season in nearby valleys, enduring some of the coldest conditions on Earth.", "Desert plants are resilient to scorching summers and frigid winters, drought, and high-salt conditions."] },
            { word: "energetic", definition: "having a lot of energy", examples: ["Traditional hula is an energetic dance.", "Energetic chants and drumming accompany the hula dancers."] },
            { word: "engage", definition: "to participate in something", examples: ["The British spend the most time vacationing outdoors in their national-trust parks, where they engage in a comfortable level of physical activity.", "Every afternoon, the children engage in outdoor activities."] },
            { word: "enhance", definition: "to improve", examples: ["Analyze its structure and alter it as necessary to enhance the outcome.", "The new features will enhance the user experience."] },
            { word: "enroll", definition: "to sign up for a class", examples: ["Travelers to Britain can enroll in courses at any of the twenty-plus adult residential colleges.", "One way to take a learning vacation is to enroll in a class."] },
            { word: "entertainment", definition: "a performance or show", examples: ["The circus is one of the oldest forms of entertainment in history.", "The circus is still a favorite form of entertainment today."] },
            { word: "environment", definition: "the natural world", examples: ["The environment needs to be protected from the effects of logging.", "Logging causes a great deal of environmental damage."] },
            { word: "epidemic", definition: "rapid spread of a disease", examples: ["In African countries where the HIV/AIDS epidemic is rampant.", "The city faced an epidemic of flu cases."] },
            { word: "equip", definition: "to provide with something", examples: ["Russia ran the first train equipped with a battery-powered electric headlamp.", "The kitchen is equipped with all the latest appliances."] },
            { word: "erosion", definition: "loss of soil from action of water or wind", examples: ["Tree roots also stabilize the soil and help prevent erosion.", "Soil erosion leads to the pollution of streams and rivers."] },
            { word: "estimate", definition: "to guess based on information", examples: ["A number that is estimated to be ten times higher in European than in African countries.", "We estimate the cost of the project to be around $1 million."] },
            { word: "evaporation", definition: "the change from liquid to gas; loss of water to the air", examples: ["P. euphratica controls evaporation by opening and closing the stomata.", "High temperatures increase the rate of evaporation."] },
            { word: "evidence", definition: "signs, proof something is or is not true", examples: ["Some scholars point to evidence suggesting that hula was traditionally danced by both men and women.", "There is no evidence to support his claim."] },
            { word: "evolve", definition: "to develop gradually", examples: ["Migration is an excellent example of how nature has responded to the biological imperative for species to evolve and spread out.", "Scientists believe that birds evolved from dinosaurs."] },
            { word: "evoke", definition: "to bring to mind", examples: ["Mention of this Pacific paradise evokes images of women in grass skirts.", "The old song evoked memories of her childhood."] },
            { word: "excavation", definition: "an area of digging, especially to find objects from past cultures", examples: ["One of the earliest clay tablets of this type was found in excavations in Mesopotamia.", "Early clay tablets and clay tokens have been found in excavations in Mesopotamia."] },
            { word: "exaggerated", definition: "made to seem more or bigger", examples: ["Actors relied on their ability to communicate thoughts and stories through facial expressions and exaggerated gestures.", "The exaggerated gestures of a mime are used for humorous effect."] },
            { word: "exhibit", definition: "something shown to the public; a display", examples: ["Other events held at the Circus Maximus included gladiator fights and exhibits of exotic animals.", "The museum has a new exhibit on ancient civilizations."] },
            { word: "exotic", definition: "unusual, from a foreign place", examples: ["Exhibits of exotic animals such as elephants and tigers.", "She loves to travel and try exotic foods."] },
            { word: "extend", definition: "to reach past, get bigger", examples: ["The effects of logging extend beyond just the felling of a swath of trees.", "The Amazon rain forest extends from Brazil into neighboring countries."] },
            { word: "extreme", definition: "very severe or difficult", examples: ["Violent sandstorms make it one of the most extreme environments on Earth.", "Many plants cannot endure the extreme heat of the desert."] },
            { word: "fascinate", definition: "to interest greatly", examples: ["Bird migration in particular has fascinated observers for centuries.", "The study of the lives of birds fascinates many people."] },
            { word: "feat", definition: "a difficult act or achievement", examples: ["Detecting and keeping track of as much motion as possible while performing physical maneuvers is quite a feat.", "Climbing Mount Everest is an incredible feat."] },
            { word: "fell", definition: "to cut down", examples: ["The logging industry fells trees to get the wood.", "They had to fell the old tree in their backyard."] },
            { word: "financial", definition: "related to money", examples: ["A successful small business starts out with proper financial support.", "He sought financial advice from an expert."] },
            { word: "flair", definition: "elegant style", examples: ["Some will do it with greater flair than others.", "The artist painted with great flair."] },
            { word: "floral", definition: "related to flowers", examples: ["The famous Hawaiian floral garlands known as leis.", "She decorated the room with floral patterns."] },
            { word: "focus", definition: "to center attention on one object; concentrate", examples: ["Focus in on something as small as a pin.", "When playing a game, always focus on the ball."] },
            { word: "found", definition: "to start or establish an institution", examples: ["The Circus Maximus was founded in Rome.", "Philip Astley founded the first modern circus."] },
            { word: "fraction", definition: "a small part", examples: ["Only a small fraction will produce the desired result.", "Only a small fraction of the population understands this complex topic."] },
            { word: "frailty", definition: "weakness and lack of strength", examples: ["Effectively portrays human frailty through physical comedy.", "Mimes can make us laugh at our own frailty."] },
            { word: "freight", definition: "cargo carried by a train, truck, or ship", examples: ["Railroad traffic became heavy enough for freight trains to delay passenger trains.", "The ship carried a large amount of freight."] },
            { word: "fringe", definition: "the edge of something", examples: ["Transitional areas between the open desert and oases on the desert fringe support diverse plant forms.", "They live on the fringe of the city."] },
            { word: "fuel", definition: "to provide energy", examples: ["About 55 percent of its body weight is made up of the fat necessary to fuel this amazing journey.", "Food fuels our bodies for daily activities."] },
            { word: "fume", definition: "harmful gas or smoke in the air", examples: ["Exhaust fumes hamper life in urban areas.", "The factory emitted toxic fumes."] },
            { word: "function", definition: "purpose, role", examples: ["It appears that one of its functions was as an astronomical observatory.", "Mythology had an important function in ancient cultures."] },
            { word: "garland", definition: "a decorative rope of flowers or leaves", examples: ["Costumes consisting of garlands of leaves.", "The winners were presented with flower garlands."] },
            { word: "generate", definition: "to make or produce", examples: ["The wind generated by the movement of the train.", "There are a variety of ways to generate electricity."] },
            { word: "gesture", definition: "a movement to express a feeling or idea", examples: ["Mimes use movements, gestures, and facial expressions to portray a character.", "He made a gesture of apology."] },
            { word: "graceful", definition: "having beauty of movement", examples: ["Swaying their hips as they perform graceful island dances.", "Modern hula is a more graceful dance."] },
            { word: "grandeur", definition: "greatness", examples: ["The circus survived to make a return to its former grandeur in the eighteenth century.", "The palace was built with great grandeur."] },
            { word: "gravity", definition: "seriousness", examples: ["Delay or diminish the gravity of conditions such as Alzheimer's disease.", "Because of the gravity of her condition, the patient was kept in the hospital."] },
            { word: "habitat", definition: "the natural area where a plant or animal lives", examples: ["Logging causes habitat loss.", "Trees provide a habitat for a myriad of animals."] },
            { word: "hamper", definition: "to make things difficult, get in the way", examples: ["Exhaust fumes hamper life in urban areas.", "Heavy rain hampered our travel plans."] },
            { word: "headquarters", definition: "central office for a military commander", examples: ["The Moscow subway was even used as a military headquarters.", "The company's headquarters is in New York City."] },
            { word: "hemisphere", definition: "one half of the Earth; also, one half of a sphere", examples: ["Migration of this type takes place primarily into the higher latitudes of the Northern Hemisphere.", "Australia is located in the Southern Hemisphere."] },
            { word: "hone", definition: "to sharpen, improve", examples: ["Trip participants hone their creative skills.", "He needs to hone his public speaking skills."] },
            { word: "humorous", definition: "funny, entertaining", examples: ["Often comedic, using body gestures and facial expressions to present a main character facing some type of conflict in a humorous way.", "The play was very humorous."] },
            { word: "illuminator", definition: "an object that produces light", examples: ["Horatio Allen's 1831 innovation, the 'Track Illuminator,' was suddenly in demand.", "An illuminator can provide an area with light."] },
            { word: "illusion", definition: "appearance of being real, false impression", examples: ["Among his well-known illusions are portrayals of a man walking against the wind.", "The magician created an amazing illusion."] },
            { word: "image", definition: "a mental picture", examples: ["Mention of this Pacific paradise evokes images of women in grass skirts.", "The word Hawaii carries images of sunny beaches."] },
            { word: "imagination", definition: "the ability to think creatively, form pictures in the mind", examples: ["The ancient ruins of Machu Picchu have captured the imaginations of travelers.", "Her vivid imagination helped her write amazing stories."] },
            { word: "impaired", definition: "damaged or weakened", examples: ["The link between aerobic exercise and improved brain function in the elderly and in people with impaired cognition.", "Impaired memory can be improved by regular exercise."] },
            { word: "impact", definition: "a strong effect", examples: ["Deforestation impacts rainfall patterns.", "The new policy had a significant impact on the economy."] },
            { word: "imperative", definition: "a priority; an urgent need", examples: ["Migration is an excellent example of how nature has responded to the biological imperative for species to evolve.", "It is our imperative to protect the natural environment."] },
            { word: "incentive", definition: "reason to do something, reward", examples: ["The government of China has offered monetary incentives to car manufacturers.", "The company offered incentives to employees who met their sales targets."] },
            { word: "indicate", definition: "to show", examples: ["Animal studies indicate that corticosteroids decrease the availability of BDNF.", "Studies indicate that exercise helps increase brainpower."] },
            { word: "indiscernibly", definition: "in a way that is impossible to see or notice", examples: ["Everything that catches the athlete's attention causes the eyes to pause almost indiscernibly.", "The changes were happening indiscernibly."] },
            { word: "indistinct", definition: "unclear", examples: ["Everything else that fills your whole area of possible sight is indistinct, lacking in detail.", "The distant voices were indistinct."] },
            { word: "industrious", definition: "hardworking", examples: ["The industrious Americans have the least vacation time.", "He is always industrious even when engaged in leisure-time activities."] },
            { word: "inevitably", definition: "certainly, to be expected", examples: ["If plans have not been made for supporting the costs of the business until it brings in a profit, inevitably it will fail.", "Mistakes will inevitably happen."] },
            { word: "influence", definition: "an effect, power", examples: ["This festival has had a significant influence on modern hula dancing.", "The influence of other cultures has changed the way hula is danced."] },
            { word: "inhabit", definition: "to live in", examples: ["Migration is the regular movement of animals between their breeding grounds and the areas that they inhabit during the rest of the year.", "Many rare species inhabit this remote island."] },
            { word: "inhibit", definition: "to prevent, slow down", examples: ["Tree shade inhibits the growth of algae.", "Roots inhibit soil erosion."] },
            { word: "initial", definition: "first, beginning", examples: ["Working alone in one's basement during the initial phases of the business.", "The initial cost of the project was higher than expected."] },
            { word: "injure", definition: "to hurt", examples: ["Travel that aims not only to avoid harming the environment, but also to make a positive contribution to the local ecology and culture.", "Large cruise ships cause several types of injury to the environment."] },
            { word: "inscribe", definition: "to mark a surface with words or letters", examples: ["The ancient Mayans are famous for the writing they inscribed on temple walls.", "The monument was inscribed with the names of fallen soldiers."] },
            { word: "institute", definition: "to start, put in place", examples: ["The Peruvian government instituted a set of restrictions on the use of the trail.", "The school plans to institute a summer program."] },
            { word: "ingredient", definition: "an item in a recipe", examples: ["Travelers may learn all about how traditional meals are prepared and what ingredients are used.", "Hard work is a key ingredient for success."] },
            { word: "innovation", definition: "a new idea or product", examples: ["Horatio Allen's 1831 innovation, the 'Track Illuminator.'", "The innovation of electric headlamps made travel much easier."] },
            { word: "intellectual", definition: "related to thinking", examples: ["Intellectual function weakens as a result of the energy expended simply sorting out the overwhelming stimuli of city life.", "Some people enjoy spending their leisure time engaged in intellectual activities."] },
            { word: "intense", definition: "very strong", examples: ["Limelights were considered too intense for trains.", "The light from candles is not very intense."] },
            { word: "intercept", definition: "to catch", examples: ["The canopy prevents surface runoff by intercepting heavy rainfall.", "The defender managed to intercept the pass."] },
            { word: "interval", definition: "the period between two times or events", examples: ["Researchers closely monitoring study participants at regular intervals.", "The train runs every 15 minutes, at regular intervals."] },
            { word: "intrinsic", definition: "basic", examples: ["Public transportation is an intrinsic part of every modern city.", "Trust is an intrinsic part of any strong relationship."] },
            { word: "investigation", definition: "a study", examples: ["Of the thousands of drugs under investigation at any one time.", "The investigation of a potential new drug costs a great deal of money."] },
            { word: "knot", definition: "a hard bump in wood", examples: ["It was a pile of pine knots burning in an iron grate.", "Burning pine knots is a way to create light."] },
            { word: "link", definition: "connection", examples: ["The link between aerobic exercise and improved brain function.", "There is a strong link between diet and health."] },
            { word: "literacy", definition: "the ability to read and write", examples: ["Today, close to three-quarters of the world's adult population can read and write, and literacy is considered a basic skill.", "Literacy was not considered necessary before modern times."] },
            { word: "literal", definition: "following the exact meaning", examples: ["Miming can be abstract, literal, or a combination of the two. Literal miming, on the other hand, tells a story.", "He took her words in a literal sense."] },
            { word: "locomotive", definition: "the engine of a train", examples: ["The car was pushed ahead of the locomotive.", "A locomotive needs a headlamp with high intensity."] },
            { word: "logging", definition: "the cutting down of trees for commercial purposes", examples: ["Environmental Impacts of Logging.", "Responsible logging practices would help protect forests."] },
            { word: "lure", definition: "to attract", examples: ["Lured by the higher salaries and better quality of life available in wealthier countries.", "Many people are lured to journalism because they are interested in traveling to other countries."] },
            { word: "luxury", definition: "something expensive and desirable but unnecessary", examples: ["This has made it a luxury accessible to fewer people.", "Hiring porters to carry your equipment on a hiking trip is quite a luxury."] },
            { word: "manufacture", definition: "to produce", examples: ["Then they manufacture compounds or take them from organisms.", "The company manufactures cars."] },
            { word: "markedly", definition: "noticeably", examples: ["Battery technology has improved markedly.", "The popularity of electric cars has grown markedly over the past few years."] },
            { word: "massive", definition: "very big", examples: ["It had become a massive marble stadium that could seat more than 200,000 spectators.", "The company made a massive profit last year."] },
            { word: "marvel", definition: "a wonderful thing", examples: ["The Inca Trail offers many marvels of its own.", "The Grand Canyon is a natural marvel."] },
            { word: "mechanism", definition: "behavior to deal with difficult situations", examples: ["Each has developed unique coping mechanisms of its own.", "The plant has a mechanism to protect itself from excessive sunlight."] },
            { word: "merely", definition: "only", examples: ["After taking a short walk on a busy city street or merely seeing pictures of city life.", "They are not merely a way to use up free time."] },
            { word: "merge", definition: "to combine", examples: ["The U.S. dance troupe Pilobolus, which merges modern dance, acrobatics, gymnastics, and mime.", "The two companies decided to merge."] },
            { word: "migration", definition: "movement from one place to another", examples: ["Migration is the regular movement of animals between their breeding grounds and the areas that they inhabit during the rest of the year.", "Bird migration generally takes place twice a year."] },
            { word: "minimize", definition: "to reduce to the least possible amount", examples: ["The plants' principal defense against these environmental stressors consists of drawing in as much water as possible while minimizing moisture loss.", "We should try to minimize waste."] },
            { word: "mode", definition: "a method or type", examples: ["Automobiles, the exciting new mode of transportation.", "What modes of transportation are commonly used in your city?"] },
            { word: "moisture", definition: "wetness or water", examples: ["The plants' principal defense against these environmental stressors consists of drawing in as much water as possible while minimizing moisture loss.", "The air in the desert has very little moisture."] },
            { word: "monetary", definition: "related to money", examples: ["The government of China has offered monetary incentives.", "Monetary reasons will cause more people to be interested in buying electric cars."] },
            { word: "monitor", definition: "to watch; observe", examples: ["Researchers closely monitoring study participants at regular intervals.", "We need to monitor the patient's condition closely."] },
            { word: "mood", definition: "a feeling, a state of mind", examples: ["Endorphins improve mood.", "People are often in a good mood after exercising."] },
            { word: "motivation", definition: "reason for doing something", examples: ["Whatever the particular motivation, certain characteristics make a small business more likely to succeed.", "Her motivation to succeed was very strong."] },
            { word: "myriad", definition: "many, numerous", examples: ["The rain forest floor, home to myriad plant life.", "There are myriad stars in the night sky."] },
            { word: "mystery", definition: "something strange, unknown, or difficult to understand", examples: ["The mysteries of the ancient culture draw thousands of tourists.", "One of the greatest mysteries of this site is the question of how it was built."] },
            { word: "mythology", definition: "set of traditional stories used to explain the origins of things", examples: ["In Chinese mythology, the creation of writing is attributed to an ancient sage.", "Mythology was very important in ancient civilizations."] },
            { word: "native", definition: "original to a place", examples: ["The name Machu Picchu means 'old peak' in the native Incan language.", "The kangaroo is native to Australia."] },
            { word: "navigation", definition: "finding the way from one place to another", examples: ["Migratory birds all have some ability to navigate and an innate drive to travel in a particular direction.", "Birds use the sun, stars, and landforms for navigation."] },
            { word: "network", definition: "a system of various parts that work together", examples: ["The ancient Inca created a network of trails throughout the mountains.", "The city has a complex network of roads."] },
            { word: "niche", definition: "a position or place that is very suitable", examples: ["Finding a niche.", "The company found its niche in the market by offering specialized products."] },
            { word: "nocturnal", definition: "active at night", examples: ["Nocturnal migrants, those species that travel at night, seem to take their navigational cues from the stars.", "Owls are nocturnal predators."] },
            { word: "nutrient", definition: "food", examples: ["Nutrients, water, and shelter for plants, animals, and microorganisms throughout the ecosystem are also lost.", "Healthy soil provides essential nutrients for plants."] },
            { word: "obesity", definition: "the condition of being very overweight", examples: ["Researchers are reporting higher levels of both stress and obesity.", "Childhood obesity and depression are reaching epidemic levels."] },
            { word: "object", definition: "a thing that can be seen or touched", examples: ["The object of the game is to score as many points as possible.", "He pointed to a strange object in the sky."] }, // Added a placeholder word for 'O'
            { word: "observer", definition: "a person who watches something", examples: ["Bird migration in particular has fascinated observers for centuries.", "If birds become aware of the presence of an observer, they quickly fly away."] },
            { word: "obscure", definition: "to make difficult to see", examples: ["When the stars are obscured by clouds, nocturnal migrants may become confused.", "Clouds obscure the view of the mountains."] },
            { word: "occupy", definition: "to be in a place; exist in", examples: ["The Taklimakan Desert occupies some 337,600 square kilometers of northwestern China.", "The old house has been occupied by the same family for generations."] },
            { word: "obvious", definition: "easy to see, clear", examples: ["Vacations are the most obvious chunk of leisure time.", "It is obvious that people need to make a deliberate decision to spend more time in nature."] },
            { word: "ongoing", definition: "continuing", examples: ["A group flies to Turkey to join an ongoing archeological dig.", "The project is an ongoing process."] },
            { word: "operation", definition: "the working of something, being used", examples: ["In its initial day of operation, the London Underground carried 30,000 passengers.", "The Paris Metro began operation in 1900."] },
            { word: "optimal", definition: "best, most favorable", examples: ["This form of migration allows birds to breed in areas that provide optimal conditions for nesting and feeding their young.", "The optimal temperature for the plant to grow is 25 degrees Celsius."] },
            { word: "outcome", definition: "result", examples: ["Analyze its structure and alter it as necessary to enhance the outcome.", "If they get the outcome that they desire and the drug cures the disease."] },
            { word: "overwhelming", definition: "overpowering; very large", examples: ["Intellectual function weakens as a result of the energy expended simply sorting out the overwhelming stimuli of city life.", "The overwhelming character of city life affects our emotions and intellectual function."] },
            { word: "particular", definition: "specific", examples: ["Whatever the particular motivation.", "He has a particular interest in ancient history."] },
            { word: "passive", definition: "not active", examples: ["Watching television, the most passive of pastimes.", "People engaged in passive pastimes don't feel rejuvenated."] },
            { word: "pastime", definition: "a free-time activity", examples: ["Watching television, the most passive of pastimes.", "His favorite pastime is building model ships."] },
            { word: "pedestrian", definition: "a person traveling on foot", examples: ["London was also home to the first underwater tunnel, a pedestrian tunnel.", "Pedestrians should use the crosswalk."] },
            { word: "performance", definition: "how well a person or machine does something", examples: ["An athlete's performance depends on training visual abilities.", "The team gave an excellent performance at last night's game."] },
            { word: "peripheral", definition: "at the edge", examples: ["We tolerate this large outlying field of blur, this peripheral view.", "Peripheral vision refers to what we see near the boundaries of our visual range."] },
            { word: "permanently", definition: "for always", examples: ["Eventually, the site was permanently retired.", "Circuses don't stay in one place permanently."] },
            { word: "personalized", definition: "made or done especially for a certain person", examples: ["A small business can offer customers personalized service.", "The gift was personalized with her name."] },
            { word: "pertain", definition: "to be related to something", examples: ["Legal requirements that pertain to the minimum wage.", "We'll explore this and other mysteries pertaining to their culture."] },
            { word: "physical", definition: "related to the body", examples: ["Spend time in nature pursuing a comfortable level of physical exercise.", "Physical activities are important for our health."] },
            { word: "pleasure", definition: "enjoyment", examples: ["Vacationers who are interested in ecotourism and still get pleasure from cruises.", "Reading is a great pleasure for me."] },
            { word: "plodding", definition: "slow", examples: ["Their plodding speed failed to capture the imagination.", "The old horse moved at a plodding pace."] },
            { word: "pollution", definition: "damage to air, water, etc.", examples: ["Logging causes pollution.", "Deforestation contributes to the effects of both air and water pollution."] },
            { word: "popular", definition: "liked by many people", examples: ["Chariot races were a popular spectator sport.", "The circus is popular all over the world."] },
            { word: "portable", definition: "easy to carry", examples: ["Making fire portable and dependable was so difficult.", "The generator did not become portable until the 1890s."] },
            { word: "portray", definition: "to represent, act out", examples: ["Mimes use movements, gestures, and facial expressions to portray a character or an emotion.", "Mimes portray common situations in humorous ways."] },
            { word: "potential", definition: "possible", examples: ["It is essential to determine who the potential customers are.", "The new employee has great potential."] },
            { word: "practice", definition: "a custom, method", examples: ["Developing practices in all aspects of daily life that preserve rather than injure the natural environment.", "Following certain practices makes damage to the environment avoidable."] },
            { word: "precisely", definition: "exactly", examples: ["All constructed from heavy blocks of granite fitted precisely together.", "We arrived at the site at precisely five o'clock."] },
            { word: "preserve", definition: "to protect, save", examples: ["To protect the natural environment and preserve the ruins.", "It is important to preserve historical buildings."] },
            { word: "previously", definition: "before", examples: ["People may have more control over their own brain health than was previously believed.", "She had previously worked as a teacher."] },
            { word: "primary", definition: "main, most important", examples: ["Nurses are often the primary caregivers.", "What were your primary reasons for choosing this profession?"] },
            { word: "principle", definition: "rule, basic idea behind a system", examples: ["Not all of these companies follow the principles of ecotourism.", "Ecotourism companies operate on the principle that avoidance of harm to the environment is not only possible but an important part of pleasure trips."] },
            { word: "product", definition: "something that is made", examples: ["The array of products or services a large business can offer.", "The company launched a new product last month."] },
            { word: "profit", definition: "money earned after paying costs", examples: ["Shows how the business will make a profit.", "The company made a huge profit this quarter."] },
            { word: "project", definition: "to estimate, calculate a future amount", examples: ["Projects the cash flow that will help the business stay afloat.", "We project that we will start earning a profit by the end of next year."] },
            { word: "prolific", definition: "producing a lot of something", examples: ["Alhagi sparsifolia represents some of the most diverse, prolific vegetation in the area.", "The author is known for his prolific writing."] },
            { word: "prominent", definition: "important, major", examples: ["Corporeal mime became the prominent form of the modern mime era.", "He is a prominent figure in the field of science."] },
            { word: "property", definition: "something that is owned", examples: ["Along with the rise of agricultural societies came the development of property ownership.", "In early agricultural societies, property consisted largely of land, livestock, and grain."] },
            { word: "prop", definition: "an object used by actors", examples: ["The art of miming grew to include acrobatics, props, and costumes.", "The stage was set with various props."] },
            { word: "publicity", definition: "activity that makes something known to the public", examples: ["The concept of ecotourism has been gaining publicity.", "The more publicity ecotourism gets, the more people will become interested."] },
            { word: "qualified", definition: "skilled, able to do a job", examples: ["Nursing schools turn down thousands of qualified applicants.", "Qualified nurses are needed everywhere."] },
            { word: "rampant", definition: "spreading out of control", examples: ["In African countries where the HIV/AIDS epidemic is rampant.", "The rampant spread of the epidemic made it difficult to control."] },
            { word: "range", definition: "the ability to see; sight", examples: ["Athletes best demonstrate just how much we can use the entire range of our vision.", "The car has a range of 300 miles on a single charge."] },
            { word: "recur", definition: "to happen or occur again", examples: ["Preventing a disease from occurring or recurring.", "The problem tends to recur every spring."] },
            { word: "recycling", definition: "collection and treatment of trash for reuse", examples: ["These efforts include recycling wastes and using fuel more efficiently.", "Recycling is important for environmental protection."] },
            { word: "reduce", definition: "to make something smaller", examples: ["Public entertainment was reduced to small bands of traveling performers.", "We need to reduce our expenses."] },
            { word: "reflector", definition: "an object that sends light back or makes it stronger", examples: ["Some trains used an oil lamp backed by a curved reflector.", "A reflector on a lamp makes the light more intense."] },
            { word: "regulate", definition: "to control", examples: ["BDNF regulates the production of synapses.", "The government regulates the banking industry."] },
            { word: "reign", definition: "the period of time that a king or queen is in power", examples: ["King David Kalakaua is credited with reviving hula dancing during his reign.", "The queen's reign lasted for many decades."] },
            { word: "release", definition: "to let something out", examples: ["Steam engines chugged under London, releasing steam through vents.", "The company will release its new product next month."] },
            { word: "reluctant", definition: "not wanting to do something; unwilling", examples: ["Parents are reluctant to let children play freely in the city.", "People can be reluctant to leave their familiar city surroundings."] },
            { word: "remnant", definition: "a small leftover piece", examples: ["Almost a century after the last remnants of the Roman Empire had vanished.", "There were only a few remnants of the old building left."] },
            { word: "remote", definition: "far away", examples: ["Companies advertise themselves as ecotourism companies, especially those that offer trips to remote, natural areas.", "They live in a remote village in the mountains."] },
            { word: "reminiscent", definition: "similar to, reminding of something", examples: ["Bip is reminiscent of both Chaplin's little tramp and Pierrot.", "The smell of pine was reminiscent of Christmas."] },
            { word: "renovation", definition: "repair or rebuilding", examples: ["It had gone through several renovations.", "The old house is undergoing a major renovation."] },
            { word: "renowned", definition: "famous", examples: ["A renowned French mime and acting teacher named Etienne Decroux.", "The city is renowned for its beautiful architecture."] },
            { word: "reputation", definition: "the general opinion about something or somebody", examples: ["A reputation for excellence in customer service.", "The restaurant has a good reputation for its food."] },
            { word: "residential", definition: "with living accommodations, related to housing", examples: ["Adult residential colleges around the country.", "Residential colleges are popular places for learning vacations."] },
            { word: "resilient", definition: "tough, able to endure difficult conditions", examples: ["Successful desert plants are resilient to scorching summers and frigid winters.", "Desert plants are resilient to heat and dryness."] },
            { word: "resort", definition: "a vacation place", examples: ["Camping out near an archeological site or sleeping in a college dormitory or youth hostel certainly costs less than staying at a luxury hotel or vacation resort.", "They spent their vacation at a ski resort."] },
            { word: "retain", definition: "to keep", examples: ["Developed countries need to invest in nursing education and focus on retaining and rewarding nurses appropriately.", "It is difficult for poorer nations to retain their nurses."] },
            { word: "revive", definition: "to bring back to life", examples: ["King David Kalakaua is credited with reviving hula dancing.", "The doctor tried to revive the patient."] },
            { word: "ritual", definition: "a set of actions used as part of a ceremony", examples: ["Hula was accompanied by great ritual.", "What was once a religious ritual has become a form of entertainment."] },
            { word: "rival", definition: "to compete with", examples: ["Paris started designing an underground rail service to rival London's.", "No other team can rival their performance."] },
            { word: "rodent", definition: "the group of small animals that includes mice and rats", examples: ["Using rodent models, researchers found increased concentrations of BDNF.", "Mice and rats are common rodents."] },
            { word: "rudimentary", definition: "basic, not well developed", examples: ["Leaving the bulk of health care up to assistant nurses, who have only rudimentary training.", "He has only a rudimentary understanding of the subject."] },
            { word: "rugged", definition: "strong; able to stand rough treatment", examples: ["Cars required more rugged parts.", "The new phone is designed to be rugged and durable."] },
            { word: "rural", definition: "related to the countryside", examples: ["The sharp division between urban and rural landscapes has been replaced by suburban sprawl.", "Life in a rural area is often peaceful."] },
            { word: "suffer", definition: "to experience something difficult or painful", examples: ["Tests demonstrate that people suffer decreases in attention span, memory, and problem-solving ability.", "When children do not spend time in nature, they suffer from obesity and depression."] },
            { word: "site", definition: "place", examples: ["The ancient ruins of Machu Picchu. The site had probably been considered a sacred place.", "This is the site of the new hospital."] },
            { word: "scan", definition: "to look over", examples: ["The athlete's view requires rapid scanning with visual focus changing rapidly.", "She scanned the newspaper headlines quickly."] },
            { word: "scholar", definition: "person who has a lot of knowledge about a particular subject", examples: ["Scholars say that writing systems developed independently.", "That scholar's specialty is ancient Mayan culture."] },
            { word: "settle", definition: "to establish a permanent place to live", examples: ["Writing systems did not develop until groups of people began settling in farming communities.", "Many immigrants settled in new countries."] },
            { word: "sharpen", definition: "to improve, perfect", examples: ["Buster Keaton and Charlie Chaplin sharpened their miming skills in the theater.", "He needs to sharpen his negotiation skills."] },
            { word: "shield", definition: "something that serves as protection", examples: ["He had devised a way of supporting the tunnel while the workers dug, called the Brunel Shield.", "The knight carried a shield to protect himself."] },
            { word: "shortage", definition: "a lack of something", examples: ["A global nursing shortage threatens to cripple health care systems worldwide.", "The nursing shortage is affecting countries around the world."] },
            { word: "showcase", definition: "a setting in which to present something", examples: ["Stalin used the stations as showcases of Russian art, culture, and engineering.", "The festival is a showcase for local talent."] },
            { word: "sound", definition: "healthy, without financial risk", examples: ["Develop a sound business plan.", "The company is in a sound financial position."] },
            { word: "spatial", definition: "of or relating to space", examples: ["A study in older humans found a correlation between aerobic fitness, the size of the hippocampus, and performance on spatial memory tests.", "Architects need good spatial reasoning skills."] },
            { word: "span", definition: "to cross", examples: ["Longer and wider bridges span rivers.", "The bridge spans the entire valley."] },
            { word: "sparse", definition: "small in numbers or amount", examples: ["Sparse rainfall, daily temperature swings of up to 20°C, and violent sandstorms make it one of the most extreme environments on Earth.", "The population in this remote area is very sparse."] },
            { word: "specialized", definition: "relating to a particular area or type of work", examples: ["In ancient times, only specialized people such as scholars, priests, or government officials used writing.", "Specialized skills are needed to identify ancient objects found in excavations."] },
            { word: "species", definition: "type; a basic group in biological classification", examples: ["Migration is an excellent example of how nature has responded to the biological imperative for species to evolve.", "There are many different species of birds in this forest."] },
            { word: "spectacular", definition: "wonderful to see", examples: ["The spectacular natural setting.", "The fireworks display was spectacular."] },
            { word: "spectator", definition: "a person who watches an event", examples: ["Chariot races were a popular spectator sport.", "Thousands of spectators gathered to watch the game."] },
            { word: "spring up", definition: "to appear", examples: ["Stores and malls have sprung up by stations.", "New businesses are springing up all over the city."] },
            { word: "sprawl", definition: "an area of spreading growth", examples: ["Suburban sprawl, town and country linked by eight-lane expressways.", "The city's sprawl extended for many miles."] },
            { word: "stabilize", definition: "to keep from changing, maintain", examples: ["Tree roots also stabilize the soil.", "We need to stabilize the damage caused by logging."] },
            { word: "standard", definition: "the normal or common thing", examples: ["Gas-fueled cars became the standard.", "This model sets a new standard for fuel efficiency."] },
            { word: "standpoint", definition: "point of view", examples: ["From that standpoint, migration of nurses from poorer to wealthier countries would appear to benefit all involved.", "From an architectural standpoint, it's a very interesting building."] },
            { word: "stave off", definition: "to prevent", examples: ["Regular exercise can help stave off some effects of normal aging.", "They worked hard to stave off the company's bankruptcy."] },
            { word: "stem", definition: "to come from, originate", examples: ["The nursing shortage largely stems from an aging population.", "His success stems from hard work and dedication."] },
            { word: "stereotype", definition: "a fixed idea people have, especially one that is wrong", examples: ["Although this image is a common stereotype of Hawaii.", "It's important not to rely on stereotypes."] },
            { word: "stimulate", definition: "to cause a response", examples: ["Factors in the brain that stimulate the growth and development of brain cells.", "The teacher tried to stimulate the students' interest in science."] },
            { word: "stray", definition: "to leave the correct route; become separated from the group", examples: ["Nocturnal migrants may become confused and return to land or stray off course.", "The dog might stray from the path if not kept on a leash."] },
            { word: "stressor", definition: "something that causes great difficulties", examples: ["The plants' principal defense against these environmental stressors.", "Lack of rain is a major stressor for desert plants."] },
            { word: "stringent", definition: "strict, firm", examples: ["The requirements for car headlamps were more stringent than those for trains.", "The company has stringent quality control standards."] },
            { word: "structure", definition: "something that is built, such as a building or bridge", examples: ["The writing they inscribed on temple walls and other religious structures.", "The ancient Maya were the creators of temples and other beautiful structures."] },
            { word: "strive", definition: "to work very hard to do something", examples: ["Ecotourists strive to have minimal impact on the places they visit.", "We must strive for excellence in everything we do."] },
            { word: "substance", definition: "material", examples: ["A substance that has no medicinal effects, known as a placebo.", "Some substances can release toxins into the blood."] },
            { word: "suburban", definition: "related to the area just outside a city", examples: ["The sharp division between urban and rural landscapes has been replaced by suburban sprawl.", "New suburban neighborhoods have developed between the cities and the rural areas."] },
            { word: "supply", definition: "the total amount available", examples: ["Doctors, too, are in short supply.", "There is a limited supply of fresh water in the desert."] },
            { word: "surface", definition: "the outer part or top of something", examples: ["The tunnels could be deeper than the original ones because electric train engines had become available. These trains did not have to be close to the surface to release steam.", "The surface of the table was smooth."] },
            { word: "survey", definition: "a study of opinions in a sample of the population", examples: ["According to surveys, close to one-third of travelers each year choose learning programs.", "The company conducted a survey to get customer feedback."] },
            { word: "survive", definition: "to continue, stay alive", examples: ["The circus survived to make a return to its former grandeur.", "The survival of the circus is due to its ability to change with the times."] },
            { word: "sway", definition: "to move back and forth", examples: ["Women in grass skirts swaying their hips.", "The trees swayed in the gentle breeze."] },
            { word: "swing", definition: "a sudden or big change", examples: ["Daily temperature swings of up to 20°C.", "After a rainstorm in the desert, there is a noticeable swing back to life."] },
            { word: "tablet", definition: "a thin, flat piece of material to write on", examples: ["A system of impressing the shapes onto clay tablets.", "One of the earliest clay tablets of this type was found in excavations."] },
            { word: "talent", definition: "a special ability", examples: ["They combined the talents of jugglers, mimes, and clowns.", "She has a great talent for music."] },
            { word: "target", definition: "to focus on", examples: ["Scientists target a step in the disease process where they believe a drug can have an effect.", "The marketing campaign targets young adults."] },
            { word: "taste", definition: "preference", examples: ["There are options to suit all tastes and budgets.", "She has excellent taste in music."] },
            { word: "terrestrial", definition: "living on the land", examples: ["Many life forms—both terrestrial and aquatic—are becoming endangered.", "Terrestrial animals include mammals, birds, reptiles, and amphibians."] },
            { word: "theoretical", definition: "abstract; based on theory", examples: ["It takes years for a drug to move from the theoretical stage to the pharmacy shelf.", "Ideas are theoretical before they are tested."] },
            { word: "thrive", definition: "to grow well", examples: ["Diverse plant forms have not only adapted to the harsh conditions but actually thrive there.", "These plants thrive in sunny conditions."] },
            { word: "tip", definition: "a piece of advice", examples: ["One important tip is to start small.", "She gave him a useful tip for studying."] },
            { word: "token", definition: "an object used to represent something else", examples: ["Originally, clay tokens of various shapes were used to count these possessions.", "The coin was a token of good luck."] },
            { word: "tolerate", definition: "to accept, allow", examples: ["We tolerate this large outlying field of blur.", "Athletes need to be able to tolerate a high level of action around them."] },
            { word: "toxic", definition: "poisonous", examples: ["Whether it has any toxic effects or by-products.", "Part of drug research involves testing for toxic effects."] },
            { word: "tradition", definition: "a custom or belief of a group of people", examples: ["Hula dancing has always been part of Hawaiian life.", "Hula is the traditional dance of Hawaii."] },
            { word: "trainer", definition: "a person who teaches skills to people or animals", examples: ["The animal trainers, clowns, and other circus performers.", "A circus animal trainer has to be able to work with exotic animals."] },
            { word: "transitional", definition: "relating to change from one type to another", examples: ["Transitional areas between the open desert and oases on the desert fringe support diverse plant forms.", "The period between childhood and adulthood is a transitional phase."] },
            { word: "tricky", definition: "difficult", examples: ["Before electricity, light was tricky business.", "This puzzle is quite tricky to solve."] },
            { word: "underground", definition: "below the ground", examples: ["London's Underground, which opened in 1863.", "They built an underwater tunnel for pedestrians below the Thames River."] },
            { word: "unconsciously", definition: "without thinking, automatically", examples: ["We unconsciously accept it.", "He unconsciously tapped his foot while thinking."] },
            { word: "unique", definition: "special, different from all others", examples: ["Has defined what is unique about the product or service it provides.", "Each person has a unique set of fingerprints."] },
            { word: "upside", definition: "advantage, good part", examples: ["The upside is that the environment and the workers are protected.", "The upside of the situation is that we learned a valuable lesson."] },
            { word: "urban", definition: "related to the city", examples: ["Exhaust fumes hamper life in urban areas.", "The sharp division between urban and rural landscapes has been replaced by suburban sprawl."] },
            { word: "utilize", definition: "to use", examples: ["All three systems are continuing to expand, providing service to more riders in more distant locales.", "We should utilize all available resources."] },
            { word: "vacancy", definition: "a position or job that needs to be filled", examples: ["Nurse migration to developed countries to help fill vacancies there.", "A vacant position at a hospital will be filled quickly."] },
            { word: "vanish", definition: "to disappear", examples: ["Many life forms are becoming endangered as forests vanish.", "The magician made the rabbit vanish."] },
            { word: "vegetation", definition: "plants", examples: ["Eighty-five percent of the Taklimakan Desert consists of shifting sand dunes, largely free of vegetation.", "Vegetation along rivers and stream banks helps maintain a steady water flow."] },
            { word: "venue", definition: "place where an event is held", examples: ["The Circus Maximus was founded in Rome as a venue for public entertainment.", "The stadium is a popular venue for concerts."] },
            { word: "vent", definition: "an opening to let air, steam, or smoke out", examples: ["Releasing steam through vents along the city streets.", "The room had a small vent for air circulation."] },
            { word: "violent", definition: "strong; sudden and destructive", examples: ["Violent sandstorms make it one of the most extreme environments on Earth.", "Violent winds tear up many plants."] },
            { word: "vision", definition: "the ability to see; sight", examples: ["Athletes best demonstrate just how much we can use the entire range of our vision.", "Good vision is important for playing sports well."] },
            { word: "vital", definition: "very important, necessary for success", examples: ["Research and planning are vital steps in setting up a small business.", "It is vital to stay hydrated in hot weather."] },
            { word: "volunteer", definition: "to work for no pay; freely offer to do something", examples: ["To learn about the natural environment and, in some cases, to volunteer on environmental protection projects.", "Many people volunteer to help out at the local animal shelter."] },
            { word: "vulnerable", definition: "weak; without defense", examples: ["Flames are vulnerable to winds and weather.", "Young birds are vulnerable to predators."] },
            { word: "wary", definition: "not completely trusting", examples: ["Travelers need to be wary and do their research carefully.", "She was wary of strangers."] },
            { word: "wilderness", definition: "natural region away from towns and cities", examples: ["Trips that involve hiking or rafting through wilderness areas.", "Many people enjoy spending time in the wilderness."] },
            { word: "windswept", definition: "unprotected from the wind", examples: ["Rock ptarmigan spend their breeding season atop windswept arctic peaks.", "The windswept coast was beautiful but cold."] }
        ];

        // Categorize words alphabetically
        const categorizedWordsAlphabetically = {};
        for (let i = 65; i <= 90; i++) { // ASCII for A-Z
            const letter = String.fromCharCode(i);
            categorizedWordsAlphabetically[letter] = allWords
                .filter(wordObj => wordObj.word.toLowerCase().startsWith(letter.toLowerCase()))
                .sort((a, b) => a.word.localeCompare(b.word)); // Sort words alphabetically within each letter category
        }

        // DOM Elements
        const categorySelect = document.getElementById('category-select');
        const flashcardSection = document.getElementById('flashcard-section');
        const flashcard = document.getElementById('flashcard');
        const flashcardWordFront = document.getElementById('flashcard-word-front');
        const flashcardWordBack = document.getElementById('flashcard-word-back');
        const flashcardDefinition = document.getElementById('flashcard-definition');
        const flashcardExamples = document.getElementById('flashcard-examples');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const progressBar = document.getElementById('progress-bar');
        const progressText = document.getElementById('progress-text');
        const startQuizBtn = document.getElementById('start-quiz-btn');
        const flashcardModeBtn = document.getElementById('flashcard-mode-btn');
        const quizSection = document.getElementById('quiz-section');
        const quizQuestion = document.getElementById('quiz-question');
        const quizOptions = document.getElementById('quiz-options');
        const quizFeedback = document.getElementById('quiz-feedback');
        const nextQuizBtn = document.getElementById('next-quiz-btn');
        const quizScore = document.getElementById('quiz-score');
        const navigationButtons = document.getElementById('navigation-buttons');

        // Modal Elements
        const customModalOverlay = document.getElementById('custom-modal-overlay');
        const modalTitle = document.getElementById('modal-title');
        const modalMessage = document.getElementById('modal-message');
        const modalCloseBtn = document.getElementById('modal-close-btn');

        // State Variables
        let currentCategoryWords = [];
        let currentWordIndex = 0;
        let isFlashcardFlipped = false;
        let quizWords = [];
        let currentQuizQuestionIndex = 0;
        let quizScoreCount = 0;
        let quizTotalQuestions = 0;

        // --- Utility Functions ---

        // Function to show custom modal
        function showModal(title, message) {
            modalTitle.textContent = title;
            modalMessage.textContent = message;
            customModalOverlay.classList.add('show');
        }

        // Function to hide custom modal
        function hideModal() {
            customModalOverlay.classList.remove('show');
        }

        // Function to shuffle an array (for quiz options)
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // --- Animated Background (Stars) ---
        function createStars() {
            const starBackground = document.getElementById('star-background');
            const numStars = 100; // Number of stars
            for (let i = 0; i < numStars; i++) {
                const star = document.createElement('div');
                star.classList.add('star');
                const size = Math.random() * 3 + 1; // Star size between 1px and 4px
                star.style.width = `${size}px`;
                star.style.height = `${size}px`;
                star.style.left = `${Math.random() * 100}%`;
                star.style.top = `${Math.random() * 100}%`;
                star.style.animationDuration = `${Math.random() * 3 + 2}s`; // Animation duration 2-5s
                star.style.animationDelay = `${Math.random() * 2}s`; // Animation delay 0-2s
                starBackground.appendChild(star);
            }
        }

        // --- Flashcard Functions ---

        // Displays the current word on the flashcard
        function displayFlashcard() {
            if (currentCategoryWords.length === 0) {
                flashcardWordFront.textContent = "No words for this letter.";
                flashcardWordBack.textContent = "";
                flashcardDefinition.textContent = "";
                flashcardExamples.innerHTML = "";
                prevBtn.disabled = true;
                nextBtn.disabled = true;
                updateProgressBar(0, 0);
                return;
            }

            const wordObj = currentCategoryWords[currentWordIndex];
            flashcardWordFront.textContent = wordObj.word;
            flashcardWordBack.textContent = wordObj.word;
            flashcardDefinition.textContent = wordObj.definition;

            const examplesList = document.createElement('ul');
            wordObj.examples.forEach(example => {
                const li = document.createElement('li');
                li.textContent = example;
                examplesList.appendChild(li);
            });
            flashcardExamples.innerHTML = ''; // Clear previous examples
            flashcardExamples.appendChild(examplesList);

            // Reset flip state
            flashcard.classList.remove('flipped');
            isFlashcardFlipped = false;

            // Update navigation button states
            prevBtn.disabled = currentWordIndex === 0;
            nextBtn.disabled = currentWordIndex === currentCategoryWords.length - 1;

            updateProgressBar(currentWordIndex + 1, currentCategoryWords.length);
        }

        // Updates the progress bar
        function updateProgressBar(current, total) {
            const percentage = total === 0 ? 0 : (current / total) * 100;
            progressBar.style.width = `${percentage}%`;
            progressText.textContent = `${current}/${total} words`;
        }

        // --- Quiz Functions ---

        // Generates quiz questions for the selected category
        function generateQuiz() {
            quizWords = shuffleArray([...currentCategoryWords]); // Shuffle words for quiz
            currentQuizQuestionIndex = 0;
            quizScoreCount = 0;
            quizTotalQuestions = quizWords.length;
            displayQuizQuestion();
            updateQuizScore();
        }

        // Displays a single quiz question
        function displayQuizQuestion() {
            quizFeedback.textContent = ''; // Clear feedback
            nextQuizBtn.disabled = true; // Disable next until answered

            if (currentQuizQuestionIndex >= quizWords.length) {
                showModal('Quiz Finished!', `You scored ${quizScoreCount} out of ${quizTotalQuestions}!`);
                // Optionally reset quiz or go back to flashcards
                flashcardModeBtn.click(); // Go back to flashcard mode
                return;
            }

            const currentWord = quizWords[currentQuizQuestionIndex];
            const questionType = Math.random() < 0.5 ? 'wordToDefinition' : 'definitionToWord'; // Random question type

            let questionText;
            let correctAnswer;
            let options = [];

            if (questionType === 'wordToDefinition') {
                questionText = `What is the definition of "${currentWord.word}"?`;
                correctAnswer = currentWord.definition;
                options.push(correctAnswer);

                // Add 3 random incorrect definitions
                while (options.length < 4) {
                    const randomWord = allWords[Math.floor(Math.random() * allWords.length)]; // Use allWords for incorrect options
                    if (randomWord.definition !== correctAnswer && !options.includes(randomWord.definition)) {
                        options.push(randomWord.definition);
                    }
                }
            } else { // definitionToWord
                questionText = `Which word means "${currentWord.definition}"?`;
                correctAnswer = currentWord.word;
                options.push(correctAnswer);

                // Add 3 random incorrect words
                while (options.length < 4) {
                    const randomWord = allWords[Math.floor(Math.random() * allWords.length)]; // Use allWords for incorrect options
                    if (randomWord.word !== correctAnswer && !options.includes(randomWord.word)) {
                        options.push(randomWord.word);
                    }
                }
            }

            shuffleArray(options); // Shuffle options

            quizQuestion.textContent = questionText;
            quizOptions.innerHTML = ''; // Clear previous options

            options.forEach(option => {
                const button = document.createElement('button');
                button.textContent = option;
                button.classList.add('px-4', 'py-2', 'rounded-md', 'text-left', 'w-full', 'mb-2', 'transition', 'duration-200');
                button.onclick = () => checkAnswer(button, option, correctAnswer);
                quizOptions.appendChild(button);
            });
        }

        // Checks the selected answer for the quiz
        function checkAnswer(selectedButton, selectedOption, correctAnswer) {
            // Disable all buttons after an answer is chosen
            Array.from(quizOptions.children).forEach(button => {
                button.disabled = true;
            });

            if (selectedOption === correctAnswer) {
                quizFeedback.textContent = 'Correct!';
                quizFeedback.classList.remove('text-red-600');
                quizFeedback.classList.add('text-green-600');
                selectedButton.classList.add('correct');
                quizScoreCount++;
            } else {
                quizFeedback.textContent = `Incorrect. The correct answer was "${correctAnswer}".`;
                quizFeedback.classList.remove('text-green-600');
                quizFeedback.classList.add('text-red-600');
                selectedButton.classList.add('incorrect');
                // Highlight the correct answer
                Array.from(quizOptions.children).forEach(button => {
                    if (button.textContent === correctAnswer) {
                        button.classList.add('correct');
                    }
                });
            }
            nextQuizBtn.disabled = false; // Enable next button
            updateQuizScore();
        }

        // Updates the quiz score display
        function updateQuizScore() {
            quizScore.textContent = `Score: ${quizScoreCount} / ${quizTotalQuestions}`;
        }

        // --- Event Listeners ---

        // Populate category dropdown on page load
        document.addEventListener('DOMContentLoaded', () => {
            // Populate with letters A-Z
            for (let i = 65; i <= 90; i++) {
                const letter = String.fromCharCode(i);
                const option = document.createElement('option');
                option.value = letter;
                option.textContent = letter;
                categorySelect.appendChild(option);
            }

            // Select the first letter (A) by default and display its words
            if (categorySelect.options.length > 0) {
                categorySelect.selectedIndex = 0;
                const selectedLetter = categorySelect.value;
                currentCategoryWords = categorizedWordsAlphabetically[selectedLetter] || [];
                displayFlashcard();
            } else {
                showModal('No Vocabulary Data', 'Please ensure vocabulary data is loaded correctly.');
            }

            createStars(); // Create animated stars
        });

        // Category selection change
        categorySelect.addEventListener('change', (event) => {
            const selectedLetter = event.target.value;
            currentCategoryWords = categorizedWordsAlphabetically[selectedLetter] || [];
            currentWordIndex = 0; // Reset index for new category
            displayFlashcard();
            // Ensure flashcard mode is active when category changes
            flashcardSection.classList.remove('hidden');
            navigationButtons.classList.remove('hidden');
            quizSection.classList.add('hidden');
            startQuizBtn.classList.remove('hidden');
            flashcardModeBtn.classList.add('hidden');
        });

        // Flashcard flip on click
        flashcard.addEventListener('click', () => {
            if (currentCategoryWords.length > 0) {
                isFlashcardFlipped = !isFlashcardFlipped;
                flashcard.classList.toggle('flipped', isFlashcardFlipped);
            }
        });

        // Previous word button
        prevBtn.addEventListener('click', () => {
            if (currentWordIndex > 0) {
                currentWordIndex--;
                displayFlashcard();
            }
        });

        // Next word button
        nextBtn.addEventListener('click', () => {
            if (currentWordIndex < currentCategoryWords.length - 1) {
                currentWordIndex++;
                displayFlashcard();
            } else {
                showModal('End of Section', 'You have reached the end of this vocabulary section. Would you like to start the quiz or review again?');
            }
        });

        // Start Quiz button
        startQuizBtn.addEventListener('click', () => {
            if (currentCategoryWords.length === 0) {
                showModal('No Words', 'Please select a letter with words to start a quiz.');
                return;
            }
            flashcardSection.classList.add('hidden');
            navigationButtons.classList.add('hidden');
            quizSection.classList.remove('hidden');
            startQuizBtn.classList.add('hidden');
            flashcardModeBtn.classList.remove('hidden');
            generateQuiz();
        });

        // Flashcard Mode button
        flashcardModeBtn.addEventListener('click', () => {
            flashcardSection.classList.remove('hidden');
            navigationButtons.classList.remove('hidden');
            quizSection.classList.add('hidden');
            startQuizBtn.classList.remove('hidden');
            flashcardModeBtn.classList.add('hidden');
            displayFlashcard(); // Show the current flashcard
        });

        // Next Quiz Question button
        nextQuizBtn.addEventListener('click', () => {
            currentQuizQuestionIndex++;
            displayQuizQuestion();
        });

        // Modal close button
        modalCloseBtn.addEventListener('click', hideModal);

    </script>
</body>
</html>
# Vocabulary
