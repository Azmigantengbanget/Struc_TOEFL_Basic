<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "The committee members ___ angry about the decision.", "id": "are" },
  { "en": "The news from the war-torn country ___ very disturbing.", "id": "is" },
  { "en": "Each of the students ___ responsible for their own project.", "id": "is" },
  { "en": "Neither the students nor the teacher ___ satisfied with the results.", "id": "is" },
  { "en": "A number of people ___ waiting outside.", "id": "are" },
  { "en": "The number of accidents ___ increased this year.", "id": "has" },
  { "en": "Everybody ___ to be respected.", "id": "wants" },
  { "en": "The dog, along with its puppies, ___ sleeping peacefully.", "id": "is" },
  { "en": "There ___ many reasons for his failure.", "id": "are" },
  { "en": "My favorite pair of jeans ___ a hole in the knee.", "id": "has" },
  { "en": "She enjoys reading, writing, and ___ to music.", "id": "listening" },
  { "en": "The new employee is diligent, efficient, and ___.", "id": "hardworking" },
  { "en": "The movie was not only entertaining ___ educational.", "id": "but also" },
  { "en": "He decided to either study medicine ___ law.", "id": "or" },
  { "en": "The report was clear, concise, and ___.", "id": "informative" },
  { "en": "They are responsible for packing their bags, checking out of the hotel, and ___ to the airport.", "id": "going" },
  { "en": "The professor advised the students to review their notes, do the practice exercises, and ___ for the exam.", "id": "prepare" },
  { "en": "The food was delicious, the service was excellent, and ___ was pleasant.", "id": "the atmosphere" },
  { "en": "The company's goals are to increase profits, expand market share, and ___ customer satisfaction.", "id": "improve" },
  { "en": "She is a talented singer, a gifted dancer, and an accomplished ___.", "id": "actress" },
  { "en": "The woman ___ I met yesterday is my new boss.", "id": "whom" },
  { "en": "This is the book ___ I was telling you about.", "id": "that" },
  { "en": "Everyone should bring ___ own lunch.", "id": "their" },
  { "en": "The cat washed ___ paws.", "id": "its" },
  { "en": "The prize will be given to ___ finishes the race first.", "id": "whoever" },
  { "en": "Between you and ___, I think this plan will fail.", "id": "me" },
  { "en": "The students, ___ were from all over the world, enjoyed the conference.", "id": "who" },
  { "en": "The company for ___ she works is very successful.", "id": "which" },
  { "en": "___ of the two options do you prefer?", "id": "Which" },
  { "en": "The book is about a man ___ loses his memory.", "id": "who" },
  { "en": "By the time I arrived, the party ___.", "id": "had already started" },
  { "en": "I ___ to the gym every day after work.", "id": "go" },
  { "en": "She ___ for the company for ten years.", "id": "has been working" },
  { "en": "While I was studying, the phone ___.", "id": "rang" },
  { "en": "If I ___ you, I would take the job.", "id": "were" },
  { "en": "He will call you as soon as he ___.", "id": "arrives" },
  { "en": "Look! The children ___ in the garden.", "id": "are playing" },
  { "en": "I wish I ___ more time to travel.", "id": "had" },
  { "en": "The Earth ___ around the Sun.", "id": "revolves" },
  { "en": "They ___ to a new house next month.", "id": "are moving" },
  { "en": "You ___ smoke in the hospital.", "id": "must not" },
  { "en": "I ___ be able to help you tomorrow.", "id": "might" },
  { "en": "___ I borrow your pen, please?", "id": "May" },
  { "en": "He ___ have studied harder for the exam.", "id": "should" },
  { "en": "She ___ be at home; her car is in the driveway.", "id": "must" },
  { "en": "When I was a child, I ___ climb trees.", "id": "could" },
  { "en": "You ___ see a doctor if you're not feeling well.", "id": "should" },
  { "en": "We ___ to finish the project by Friday.", "id": "have" },
  { "en": "They ___ have left already; it's still early.", "id": "can't" },
  { "en": "He ___ be late for the meeting.", "id": "shouldn't" },
  { "en": "I saw ___ eagle soaring in the sky.", "id": "an" },
  { "en": "___ moon is very bright tonight.", "id": "The" },
  { "en": "She is studying to be ___ doctor.", "id": "a" },
  { "en": "I need to buy ___ new pair of shoes.", "id": "a" },
  { "en": "There are many ___ in the library.", "id": "books" },
  { "en": "The government is responsible for ___ welfare of its citizens.", "id": "the" },
  { "en": "I would like to have ___ cup of coffee.", "id": "a" },
  { "en": "___ information you gave me was very helpful.", "id": "The" },
  { "en": "She has a lot of ___.", "id": "experience" },
  { "en": "He gave me some good ___.", "id": "advice" },
  { "en": "The music is too ___.", "id": "loud" },
  { "en": "He speaks English very ___.", "id": "well" },
  { "en": "The cake tastes ___.", "id": "delicious" },
  { "en": "She drives her car very ___.", "id": "carefully" },
  { "en": "The patient is feeling ___ better today.", "id": "much" },
  { "en": "I have ___ seen that movie before.", "id": "never" },
  { "en": "The ___ dog barked all night.", "id": "noisy" },
  { "en": "He is a ___ talented musician.", "id": "highly" },
  { "en": "The children were playing ___ in the park.", "id": "happily" },
  { "en": "The report is ___ complete.", "id": "almost" },
  { "en": "The book is ___ the table.", "id": "on" },
  { "en": "I will see you ___ Monday.", "id": "on" },
  { "en": "The meeting is ___ 10 AM.", "id": "at" },
  { "en": "He lives ___ a small village.", "id": "in" },
  { "en": "She is afraid ___ spiders.", "id": "of" },
  { "en": "I am interested ___ learning a new language.", "id": "in" },
  { "en": "He is responsible ___ the marketing department.", "id": "for" },
  { "en": "The cat is hiding ___ the bed.", "id": "under" },
  { "en": "The store is open ___ 9 AM to 9 PM.", "id": "from" },
  { "en": "I am looking forward ___ seeing you.", "id": "to" },
  { "en": "I wanted to go to the party, ___ I was too tired.", "id": "but" },
  { "en": "She studied hard, ___ she passed the exam easily.", "id": "so" },
  { "en": "I will call you ___ I get home.", "id": "when" },
  { "en": "___ it was raining, we still went for a walk.", "id": "Although" },
  { "en": "He is not only a great leader ___ a kind person.", "id": "but also" },
  { "en": "You can have either tea ___ coffee.", "id": "or" },
  { "en": "I need to finish my homework ___ I can watch TV.", "id": "before" },
  { "en": "She has been working here ___ she graduated from college.", "id": "since" },
  { "en": "Never ___ I seen such a beautiful sunset.", "id": "have" },
  { "en": "Not only ___ he a great singer, but he is also a talented actor.", "id": "is" },
  { "en": "Little ___ she know that he was planning a surprise party for her.", "id": "did" },
  { "en": "On no occasion ___ the company break its promises to its customers.", "id": "did" },
  { "en": "Seldom ___ we see such a magnificent display of fireworks.", "id": "do" },
  { "en": "So ___ was the movie that I saw it twice.", "id": "good" },
  { "en": "Such ___ his strength that he could lift the heavy rock.", "id": "was" },
  { "en": "Had I known you were coming, I ___ have baked a cake.", "id": "would" },
  { "en": "Only after the exam ___ I realize how much I had forgotten.", "id": "did" },
  { "en": "No sooner ___ the sun risen than the birds began to sing.", "id": "had" },
  { "en": "The hotel ___ we stayed last summer was excellent.", "id": "where" },
  { "en": "I don't know ___ he decided to leave so suddenly.", "id": "why" },
  { "en": "She will be late ___ she gets stuck in traffic.", "id": "if" },
  { "en": "The man ___ in the corner is my uncle.", "id": "sitting" },
  { "en": "The ideas ___ in the report were revolutionary.", "id": "presented" },
  { "en": "___ by the good news, she called her family immediately.", "id": "Excited" },
  { "en": "This book is ___ than the last one I read.", "id": "more interesting" },
  { "en": "She is the ___ student in the entire class.", "id": "smartest" },
  { "en": "He runs ___ than his brother.", "id": "faster" },
  { "en": "Of the three options, this one is the ___ expensive.", "id": "least" },
  { "en": "If he had taken my advice, he ___ in trouble now.", "id": "would not be" },
  { "en": "I would have gone to the party if I ___ invited.", "id": "had been" },
  { "en": "If you ___ water, it boils.", "id": "heat" },
  { "en": "___ you see him, please give him this message.", "id": "Should" },
  { "en": "She would travel more if she ___ more vacation time.", "id": "had" },
  { "en": "I can't help ___ at his jokes.", "id": "laughing" },
  { "en": "He promised ___ me with my homework.", "id": "to help" },
  { "en": "She is considering ___ to a new city.", "id": "moving" },
  { "en": "It is important ___ the instructions carefully.", "id": "to read" },
  { "en": "He avoids ___ in heavy traffic.", "id": "driving" },
  { "en": "The manager had the assistant ___ the report.", "id": "type" },
  { "en": "My parents made me ___ my room every weekend.", "id": "clean" },
  { "en": "She got her husband ___ the groceries.", "id": "to buy" },
  { "en": "The teacher let the students ___ the classroom early.", "id": "leave" },
  { "en": "I had my car ___ by a professional mechanic.", "id": "repaired" },
  { "en": "Dr. Evans, ___ of the chemistry department, will give the opening speech.", "id": "the head" },
  { "en": "The book, ___, was an international bestseller.", "id": "a novel by a first-time author" },
  { "en": "We visited Stratford-upon-Avon, ___ of William Shakespeare.", "id": "the birthplace" },
  { "en": "The platypus, ___, is native to Australia.", "id": "a semi-aquatic mammal" },
  { "en": "The new policy will ___ everyone in the company.", "id": "affect" },
  { "en": "The medicine had a positive ___ on her health.", "id": "effect" },
  { "en": "There are ___ books on the table than on the shelf.", "id": "fewer" },
  { "en": "I need to ___ down for a few minutes.", "id": "lie" },
  { "en": "Please ___ the book on the desk.", "id": "lay" },
  { "en": "The flock of birds ___ south for the winter.", "id": "flies" },
  { "en": "The data ___ that the new strategy is effective.", "id": "show" },
  { "en": "One of the main reasons for his success ___ his perseverance.", "id": "is" },
  { "en": "Mathematics ___ a subject I've always enjoyed.", "id": "is" },
  { "en": "The police ___ investigating the incident.", "id": "are" },
  { "en": "It was ___ a difficult test that few students passed.", "id": "such" },
  { "en": "Not until the next day ___ she realize her mistake.", "id": "did" },
  { "en": "The new regulations require that every employee ___ a training course.", "id": "attend" },
  { "en": "___ of the rain, the game was not canceled.", "id": "In spite" },
  { "en": "He is known for his ability ___ complex problems.", "id": "to solve" },
  { "en": "The higher the temperature, ___ the water evaporates.", "id": "the faster" },
  { "en": "The equipment, ___ was purchased last year, is already obsolete.", "id": "which" },
  { "en": "He insisted on ___ for the entire meal.", "id": "paying" },
  { "en": "Rarely ___ such a talented performer on this stage.", "id": "have we seen" },
  { "en": "___ to get a promotion, she has been working extra hours.", "id": "Hoping" },
  { "en": "That is the man ___ car was stolen.", "id": "whose" },
  { "en": "I would rather ___ at home than go out tonight.", "id": "stay" },
  { "en": "All of the furniture in the house ___ old.", "id": "is" },
  { "en": "___ he is very rich, he is not a happy man.", "id": "Although" },
  { "en": "The project must be finished ___ of the cost.", "id": "regardless" },
  { "en": "The committee is made ___ of ten members.", "id": "up" },
  { "en": "She is accustomed ___ with a team.", "id": "to working" },
  { "en": "Hardly ___ I walked in the door when the phone rang.", "id": "had" },
  { "en": "He has little experience, ___ he got the job.", "id": "yet" },
  { "en": "The final decision is ___ to the manager.", "id": "up" },
  { "en": "The old building was torn ___ to make way for a new one.", "id": "down" },
  { "en": "Neither of the answers ___ correct.", "id": "is" },
  { "en": "The lecture was so ___ that I almost fell asleep.", "id": "boring" },
  { "en": "Having ___ the book, he could answer all the questions.", "id": "read" },
  { "en": "He is as tall ___ his father.", "id": "as" },
  { "en": "The company plans on ___ more employees next year.", "id": "hiring" },
  { "en": "This is ___ the best movie I have ever seen.", "id": "by far" },
  { "en": "She is capable ___ handling the situation herself.", "id": "of" },
  { "en": "The amount of information ___ overwhelming.", "id": "was" },
  { "en": "The faster he drove, ___ he became.", "id": "the more nervous" },
  { "en": "It's no use ___ about the past.", "id": "worrying" },
  { "en": "The politician's speech had a strong ___ on the audience.", "id": "influence" },
  { "en": "___ the cold weather, she went out without a coat.", "id": "Despite" },
  { "en": "The jury ___ unable to agree on a verdict.", "id": "was" },
  { "en": "She found the key ___ under the mat.", "id": "hidden" },
  { "en": "He objected ___ treated like a child.", "id": "to being" },
  { "en": "A large percentage of the students ___ from out of state.", "id": "come" },
  { "en": "He works too ___ and needs to take a break.", "id": "hard" },
  { "en": "The news about the merger ___ announced yesterday.", "id": "was" },
  { "en": "That is a matter ___ which we have little control.", "id": "over" },
  { "en": "Her job involves ___ with clients from all over the world.", "id": "dealing" },
  { "en": "The discovery was ___ to years of dedicated research.", "id": "due" },
  { "en": "They were disappointed ___ the results of the experiment.", "id": "with" },
  { "en": "___ of his warnings, they went ahead with the plan.", "id": "Regardless" },
  { "en": "The more I think about it, ___ I understand.", "id": "the less" },
  { "en": "___ its small size, the device is very powerful.", "id": "For" },
  { "en": "The witness's testimony was inconsistent ___ the evidence.", "id": "with" },
  { "en": "Scarcely had she finished speaking ___ the audience burst into applause.", "id": "when" },
  { "en": "I am not used ___ up so early in the morning.", "id": "to getting" },
  { "en": "___ a talented artist, he is also a successful businessman.", "id": "Besides being" },
  { "en": "His health has improved ___ since he started exercising.", "id": "significantly" },
  { "en": "The two companies are ___ to merge by the end of the year.", "id": "expected" },
  { "en": "The defendant was accused ___ lying under oath.", "id": "of" },
  { "en": "He found it difficult ___ his true feelings.", "id": "to express" },
  { "en": "The quality of the products ___ decreased over the years.", "id": "has" },
  { "en": "The professor recommended that he ___ his paper again.", "id": "write" },
  { "en": "It is essential that all passengers ___ their seatbelts.", "id": "fasten" },
  { "en": "The law requires that every citizen ___ taxes.", "id": "pay" },
  { "en": "Her boss insisted that she ___ the meeting.", "id": "attend" },
  { "en": "It is vital that you ___ on time.", "id": "be" },
  { "en": "So confusing ___ the instructions that nobody understood them.", "id": "were" },
  { "en": "Such ___ the force of the storm that trees were uprooted.", "id": "was" },
  { "en": "Were I to win the lottery, I ___ a new house.", "id": "would buy" },
  { "en": "Only by working hard ___ you succeed.", "id": "will" },
  { "en": "At no time ___ the doors be left unlocked.", "id": "should" },
  { "en": "The problem is ___ to be more serious than first thought.", "id": "considered" },
  { "en": "He remembers ___ to the zoo as a child.", "id": "being taken" },
  { "en": "New employees must ___ a security check.", "id": "be given" },
  { "en": "The proposal is currently ___ reviewed by the board.", "id": "being" },
  { "en": "Having ___ about the opportunity, he applied immediately.", "id": "been told" },
  { "en": "After finishing the report, ___ was sent to the manager.", "id": "it" },
  { "en": "Walking down the street, ___ caught my eye.", "id": "a beautiful display" },
  { "en": "To apply for this job, ___ must be submitted by Friday.", "id": "an application" },
  { "en": "While still in college, ___ on his first novel.", "id": "he began working" },
  { "en": "The team's success can be attributed ___ their hard work.", "id": "to" },
  { "en": "She prides herself ___ her ability to speak three languages.", "id": "on" },
  { "en": "The new law will go ___ effect next month.", "id": "into" },
  { "en": "He was preoccupied ___ his own thoughts.", "id": "with" },
  { "en": "The curriculum is designed to be compatible ___ the needs of the students.", "id": "with" },
  { "en": "___ he said was not true.", "id": "What" },
  { "en": "___ the team will win the championship is uncertain.", "id": "Whether" },
  { "en": "___ is responsible for the damage must pay for the repairs.", "id": "Whoever" },
  { "en": "___ the Earth is round is a well-known fact.", "id": "That" },
  { "en": "He enjoys classical music, ___ his wife prefers jazz.", "id": "whereas" },
  { "en": "You can borrow the car, ___ that you return it by 6 PM.", "id": "provided" },
  { "en": "The research is important ___ it provides a new perspective on the issue.", "id": "inasmuch as" },
  { "en": "He must be smart; ___, he wouldn't have been accepted into that university.", "id": "otherwise" },
  { "en": "She worked two jobs ___ she could save money for college.", "id": "so that" },
  { "en": "There is ___ hope of finding any survivors.", "id": "little" },
  { "en": "___ of the participants was given a certificate.", "id": "Each" },
  { "en": "He has made ___ mistakes in his report, so he needs to revise it.", "id": "a few" },
  { "en": "He has very ___ patience for incompetence.", "id": "little" },
  { "en": "There isn't ___ sugar left; we need to buy some.", "id": "much" },
  { "en": "The report was ___ and to the point.", "id": "concise" },
  { "en": "The ancient ruins were discovered ___ by accident.", "id": "purely" },
  { "en": "His argument was not very ___.", "id": "convincing" },
  { "en": "The results of the study were ___ with previous findings.", "id": "consistent" },
  { "en": "For a ___ of reasons, the project was delayed.", "id": "variety" },
  { "en": "The city is known for its ___ architecture.", "id": "diverse" },
  { "en": "It is highly ___ that he will accept the offer.", "id": "unlikely" },
  { "en": "The changes were implemented ___ a three-month period.", "id": "over" },
  { "en": "The two theories are ___ exclusive.", "id": "mutually" },
  { "en": "The new system is far superior ___ the old one.", "id": "to" },
  { "en": "It is ___ knowledge that the company is in financial trouble.", "id": "common" },
  { "en": "The contract is ___ to renewal in December.", "id": "subject" },
  { "en": "The senator's speech was intended to ___ support for the new bill.", "id": "generate" },
  { "en": "They failed to take ___ account the changing market conditions.", "id": "into" },
  { "en": "The witness was unable to ___ the suspect in the lineup.", "id": "identify" },
  { "en": "The machine is complex; ___, its operation is quite simple.", "id": "however" },
  { "en": "His decision was based ___ on the financial report.", "id": "solely" },
  { "en": "The museum has a ___ collection of modern art.", "id": "vast" },
  { "en": "She has a tendency ___ things until the last minute.", "id": "to postpone" },
  { "en": "The company is committed ___ providing excellent customer service.", "id": "to" },
  { "en": "The rules are applicable ___ all employees, without exception.", "id": "to" },
  { "en": "The investigation brought several ___ facts to light.", "id": "disturbing" },
  { "en": "He was praised for his ___ handling of the crisis.", "id": "adept" },
  { "en": "This software is not ___ with your operating system.", "id": "compatible" },
  { "en": "The company's profits have remained ___ for the past two years.", "id": "stable" },
  { "en": "He has an ___ for detail that makes him a great editor.", "id": "eye" },
  { "en": "Despite his initial reluctance, he ___ agreed to help.", "id": "eventually" },
  { "en": "A thorough understanding of the topic is a ___ for this course.", "id": "prerequisite" },
  { "en": "The new building will ___ the university's library and classrooms.", "id": "house" },
  { "en": "The goal is to make the information easily ___ to everyone.", "id": "accessible" },
  { "en": "His work is ___ of the best research in the field.", "id": "representative" },
  { "en": "The government has launched a new ___ to combat unemployment.", "id": "initiative" },
  { "en": "The two countries agreed to ___ in the fields of science and technology.", "id": "cooperate" },
  { "en": "The instructions were ___ and difficult to follow.", "id": "ambiguous" },
  { "en": "She was chosen for the job on the ___ of her experience.", "id": "basis" },
  { "en": "He had to ___ his appointment with the doctor.", "id": "cancel" },
  { "en": "The effects of the new law have yet to be ___.", "id": "determined" },
  { "en": "It is the responsibility of the government to ___ its citizens.", "id": "protect" },
  { "en": "The speaker's comments were ___ to the topic of the conference.", "id": "irrelevant" },
  { "en": "He is widely ___ as one of the best writers of his generation.", "id": "regarded" },
  { "en": "The committee is comprised ___ experts from various fields.", "id": "of" },
  { "en": "The company has a strict policy ___ lateness.", "id": "regarding" },
  { "en": "___ to popular belief, the Earth is not a perfect sphere.", "id": "Contrary" },
  { "en": "The chemical is ___ to the environment.", "id": "harmful" },
  { "en": "The evidence presented was not sufficient ___ a conviction.", "id": "for" },
  { "en": "The author provides a ___ analysis of the historical event.", "id": "detailed" },
  { "en": "There has been a ___ decline in the company's profits.", "id": "steady" },
  { "en": "The purpose of the meeting is to ___ a solution to the problem.", "id": "find" },
  { "en": "He is ___ to winning the competition this year.", "id": "dedicated" },
  { "en": "The data is stored ___ on a secure server.", "id": "electronically" },
  { "en": "He has a reputation ___ being difficult to work with.", "id": "for" },
  { "en": "The factory was shut down ___ to safety concerns.", "id": "due" },
  { "en": "The new research has ___ implications for the future of medicine.", "id": "profound" },
  { "en": "It is ___ for parents to support their children.", "id": "natural" },
  { "en": "This approach is fundamentally ___ from the previous one.", "id": "different" },
  { "en": "She adapted ___ to her new environment.", "id": "quickly" },
  { "en": "His duties ___ of managing the team and reporting to the director.", "id": "consist" },
  { "en": "The book provides a critical ___ of the government's policies.", "id": "assessment" },
  { "en": "The organization works to ___ the rights of children.", "id": "promote" },
  { "en": "The negotiations ___ in a mutually beneficial agreement.", "id": "resulted" },
  { "en": "The research team will ___ a study on the effects of climate change.", "id": "carry out" },
  { "en": "Before making a decision, we need to ___ all the available options.", "id": "look into" },
  { "en": "The speaker ___ that the new policy would have significant consequences.", "id": "pointed out" },
  { "en": "The meeting has been ___ until next Friday.", "id": "put off" },
  { "en": "Scientists are trying to ___ a cure for the disease.", "id": "come up with" },
  { "en": "The new software is not only efficient but also ___.", "id": "easy to use" },
  { "en": "You can ___ pay by credit card or cash.", "id": "either" },
  { "en": "Neither the manager ___ the employees were happy with the outcome.", "id": "nor" },
  { "en": "The course covers ___ theoretical concepts and practical applications.", "id": "both" },
  { "en": "The goal is not to punish but ___ behavior.", "id": "to correct" },
  { "en": "Her new car is the same color ___ her old one.", "id": "as" },
  { "en": "This winter is not ___ cold as last year's.", "id": "so" },
  { "en": "His qualifications are different ___ what the job requires.", "id": "from" },
  { "en": "The more he read, ___ he understood the complexity of the issue.", "id": "the more" },
  { "en": "He speaks English ___ a native speaker.", "id": "like" },
  { "en": "The country is experiencing rapid ___ growth.", "id": "economic" },
  { "en": "For the ___ of the environment, we should recycle more.", "id": "protection" },
  { "en": "The artist is known for her ___ use of color.", "id": "creative" },
  { "en": "He explained the process very ___.", "id": "clearly" },
  { "en": "The ___ of the new policy has been widely debated.", "id": "effectiveness" },
  { "en": "He went to the meeting ___ feeling very ill.", "id": "despite" },
  { "en": "She studied hard ___ pass the exam.", "id": "in order to" },
  { "en": "___ the fact that he was tired, he continued working.", "id": "In spite of" },
  { "en": "The company lowered its prices ___ attract more customers.", "id": "so as to" },
  { "en": "He is a great leader, ___ his lack of experience.", "id": "notwithstanding" },
  { "en": "Every student must submit ___ assignment by Friday.", "id": "their" },
  { "en": "The company will announce ___ decision next week.", "id": "its" },
  { "en": "The team members celebrated ___ victory.", "id": "their" },
  { "en": "The professor, along with his students, presented ___ research at the conference.", "id": "his" },
  { "en": "If anyone calls, please take a message for ___.", "id": "them" },
  { "en": "___ Lake Superior is the largest of the Great Lakes.", "id": "(no article)" },
  { "en": "He is studying the history of ___ United Kingdom.", "id": "the" },
  { "en": "___ honesty is a valuable quality.", "id": "(no article)" },
  { "en": "She plays ___ piano beautifully.", "id": "the" },
  { "en": "They traveled to ___ Philippines for their vacation.", "id": "the" },
  { "en": "If I had more time, I ___ join a gym.", "id": "could" },
  { "en": "If he had studied more, he ___ have passed the exam.", "id": "might" },
  { "en": "If you see any errors, you ___ let me know immediately.", "id": "should" },
  { "en": "She ___ have been on time if the traffic hadn't been so bad.", "id": "would" },
  { "en": "If it were not for his help, we ___ still be working on this project.", "id": "would" },
  { "en": "The plan was rejected on the ___ that it was too expensive.", "id": "grounds" },
  { "en": "The two artists have ___ different styles.", "id": "distinctly" },
  { "en": "He made a ___ effort to improve his work.", "id": "conscious" },
  { "en": "The government must address the ___ needs of its citizens.", "id": "fundamental" },
  { "en": "The new evidence ___ the defendant's story.", "id": "contradicts" },
  { "en": "Her contributions to the field of medicine are ___.", "id": "undeniable" },
  { "en": "The process is ___ simple, but it requires precision.", "id": "relatively" },
  { "en": "The factory must ___ with strict environmental regulations.", "id": "comply" },
  { "en": "His theory is based on the ___ that all people are rational.", "id": "assumption" },
  { "en": "The company is seeking to ___ its operations into Asia.", "id": "expand" },
  { "en": "The primary ___ of this study is to test the hypothesis.", "id": "objective" },
  { "en": "The politician's comments ___ a great deal of controversy.", "id": "sparked" },
  { "en": "We have ___ reason to believe that the project will be successful.", "id": "every" },
  { "en": "The issue is far more ___ than it appears at first glance.", "id": "complex" },
  { "en": "He is an ___ supporter of environmental protection.", "id": "avid" },
  { "en": "The book gives a fascinating ___ into the culture of ancient Rome.", "id": "insight" },
  { "en": "The new drug has not yet been approved for ___ use.", "id": "widespread" },
  { "en": "The company is known for its ___ to quality.", "id": "commitment" },
  { "en": "The evidence is ___ to prove his guilt beyond a reasonable doubt.", "id": "insufficient" },
  { "en": "The defendant was ___ of all charges.", "id": "acquitted" },
  { "en": "The device can ___ slight changes in temperature.", "id": "detect" },
  { "en": "They reached a ___ agreement after weeks of negotiation.", "id": "compromise" },
  { "en": "The rise in sea levels is a direct ___ of global warming.", "id": "consequence" },
  { "en": "His work has had a ___ impact on the industry.", "id": "lasting" },
  { "en": "The system was designed to be ___ with existing technology.", "id": "integrated" },
  { "en": "It is ___ that all staff attend the safety training.", "id": "mandatory" },
  { "en": "The project was completed ahead of ___.", "id": "schedule" },
  { "en": "She has a ___ understanding of the subject.", "id": "comprehensive" },
  { "en": "The two departments often ___ on joint projects.", "id": "collaborate" },
  { "en": "His promotion was a ___ of his hard work and dedication.", "id": "reflection" },
  { "en": "The organization provides ___ aid to developing countries.", "id": "humanitarian" },
  { "en": "He was ___ to accept the award on behalf of his team.", "id": "honored" },
  { "en": "The study's findings have been ___ by several other researchers.", "id": "validated" },
  { "en": "The castle is perched ___ on a steep cliff.", "id": "precariously" },
  { "en": "The company has a ___ advantage over its competitors.", "id": "significant" },
  { "en": "The library offers a quiet ___ for studying.", "id": "environment" },
  { "en": "The volcano is ___, but it could erupt at any time.", "id": "dormant" },
  { "en": "The new manager hopes to ___ better communication within the team.", "id": "foster" },
  { "en": "The experiment was designed to ___ a specific theory.", "id": "test" },
  { "en": "He is an expert in the ___ of international relations.", "id": "field" },
  { "en": "The government plans to ___ the new tax policy next year.", "id": "implement" },
  { "en": "The painting is an excellent ___ of the artist's early style.", "id": "example" },
  { "en": "The company is ___ for its innovative products.", "id": "renowned" },
  { "en": "She has a ___ talent for music.", "id": "remarkable" },
  { "en": "The doctor gave him a ___ examination.", "id": "thorough" },
  { "en": "The conference will ___ on topics related to renewable energy.", "id": "focus" },
  { "en": "His theory is not ___ by the available evidence.", "id": "supported" },
  { "en": "The ___ of the rainforest is a global concern.", "id": "destruction" },
  { "en": "The two systems are ___ in many ways, but they also have key differences.", "id": "similar" },
  { "en": "The project is a ___ venture between two leading companies.", "id": "joint" },
  { "en": "They had to overcome ___ obstacles to achieve their goal.", "id": "numerous" },
  { "en": "The law is intended to protect ___ species.", "id": "endangered" },
  { "en": "The results were ___ and required further investigation.", "id": "inconclusive" },
  { "en": "She has made ___ progress in her recovery.", "id": "substantial" },
  { "en": "He gave a ___ account of what had happened.", "id": "factual" },
  { "en": "The city has undergone a ___ transformation in the last decade.", "id": "radical" },
  { "en": "The program is designed to be ___ to the needs of each student.", "id": "adaptable" },
  { "en": "The ___ of the study were published in a scientific journal.", "id": "findings" },
  { "en": "He is considered a ___ figure in modern art.", "id": "pivotal" },
  { "en": "The new vaccine is ___ to be highly effective.", "id": "believed" },
  { "en": "The tour guide gave us some interesting ___ about the history of the castle.", "id": "information" },
  { "en": "We don't have ___ luggage, just a couple of small bags.", "id": "much" },
  { "en": "The number of ___ applying for the job has increased significantly.", "id": "people" },
  { "en": "The company has invested in new ___ to improve production.", "id": "equipment" },
  { "en": "These strange ___ have been observed in the night sky.", "id": "phenomena" },
  { "en": "She bought a ___ Italian leather jacket.", "id": "beautiful black" },
  { "en": "He lives in a ___ house by the sea.", "id": "lovely little old" },
  { "en": "They found a ___ wooden box hidden under the floorboards.", "id": "small square" },
  { "en": "___ no further questions, the meeting was adjourned.", "id": "There being" },
  { "en": "___ from a distance, the city looks like a collection of tiny lights.", "id": "Seen" },
  { "en": "___ his keys, he could not get into his apartment.", "id": "Having lost" },
  { "en": "___ what to do, she asked for advice.", "id": "Not knowing" },
  { "en": "The problem, ___ unresolved, continued to cause issues.", "id": "left" },
  { "en": "___ a new language can be a challenging but rewarding experience.", "id": "Learning" },
  { "en": "He went to the library ___ a book.", "id": "to borrow" },
  { "en": "I look forward to ___ from you soon.", "id": "hearing" },
  { "en": "The manager postponed ___ a decision until more information was available.", "id": "making" },
  { "en": "She is talented at ___ stories.", "id": "telling" },
  { "en": "She ___ arrives at the office before 9 AM.", "id": "always" },
  { "en": "He drove ___ to avoid the potholes in the road.", "id": "carefully" },
  { "en": "I have ___ finished my homework; I just have one question left.", "id": "almost" },
  { "en": "He has ___ traveled outside of his home country.", "id": "never" },
  { "en": "The company performed well last quarter; ___, its stock price increased.", "id": "consequently" },
  { "en": "The plan is well-designed. ___, it requires a significant investment.", "id": "Nevertheless" },
  { "en": "The historical evidence is clear. ___, the new findings support the theory.", "id": "Furthermore" },
  { "en": "He did not study for the test; ___, he failed.", "id": "hence" },
  { "en": "My car, ___ I bought last year, has been very reliable.", "id": "which" },
  { "en": "This is the only one of his books ___ has been translated into English.", "id": "that" },
  { "en": "Summer is the season ___ many people go on vacation.", "id": "when" },
  { "en": "I visited the town ___ my father grew up.", "id": "where" },
  { "en": "It ___ that the new policy will be announced next week.", "id": "is expected" },
  { "en": "The fugitive is believed ___ hiding in the mountains.", "id": "to be" },
  { "en": "It was ___ that the company would merge with its competitor.", "id": "rumored" },
  { "en": "Many historical artifacts were thought ___ lost forever.", "id": "to have been" },
  { "en": "She is held in high ___ by all of her colleagues.", "id": "regard" },
  { "en": "We need to ___ advantage of this opportunity.", "id": "take" },
  { "en": "Her opinion is irrelevant ___ the matter at hand.", "id": "to" },
  { "en": "I started working here two years ___.", "id": "ago" },
  { "en": "She has been studying English ___ five years.", "id": "for" },
  { "en": "He hasn't seen his family ___ last Christmas.", "id": "since" },
  { "en": "They lived in that house ___ a long time before moving.", "id": "for" },
  { "en": "It has been a year ___ I last saw him.", "id": "since" },
  { "en": "The amount of work he does is ___.", "id": "amazing" },
  { "en": "The evidence was convincing; ___, the jury found him guilty.", "id": "therefore" },
  { "en": "The more you ignore the problem, ___ it will become.", "id": "the worse" },
  { "en": "It is difficult to ___ the effects of the new law at this stage.", "id": "assess" },
  { "en": "The university is known for its high ___ standards.", "id": "academic" },
  { "en": "He has a ___ grasp of the main issues.", "id": "firm" },
  { "en": "The region is ___ to earthquakes.", "id": "prone" },
  { "en": "The two companies operate independently of ___ another.", "id": "one" },
  { "en": "She has made a ___ contribution to the project.", "id": "valuable" },
  { "en": "His comments were not ___ for the situation.", "id": "appropriate" },
  { "en": "___ of the bad weather, the flight was delayed.", "id": "Because" },
  { "en": "The purpose of the research is ___ a new theory.", "id": "to develop" },
  { "en": "I am not accustomed to ___ in such a hot climate.", "id": "living" },
  { "en": "The discovery of penicillin was a ___ in the history of medicine.", "id": "milestone" },
  { "en": "The government took measures to ___ the economy.", "id": "stimulate" },
  { "en": "The artist's work is on ___ at the local gallery.", "id": "display" },
  { "en": "The negotiations broke ___ without an agreement.", "id": "down" },
  { "en": "He is ___ regarded as an expert in his field.", "id": "highly" },
  { "en": "The law applies to all citizens, ___ of their age or status.", "id": "regardless" },
  { "en": "He is fluent ___ both Spanish and Portuguese.", "id": "in" },
  { "en": "The study was limited in ___ because of its small sample size.", "id": "scope" },
  { "en": "The final exam will account ___ 40% of the total grade.", "id": "for" },
  { "en": "The new product is designed to ___ the needs of modern consumers.", "id": "meet" },
  { "en": "His work is a testament ___ his dedication and talent.", "id": "to" },
  { "en": "The city's infrastructure is in ___ need of an upgrade.", "id": "dire" },
  { "en": "She was unable to attend the meeting ___ a prior commitment.", "id": "due to" },
  { "en": "The two countries have strong economic ___.", "id": "ties" },
  { "en": "The company has ___ on a new marketing strategy.", "id": "decided" },
  { "en": "The professor's explanation was ___ and easy to understand.", "id": "lucid" },
  { "en": "He is responsible for ___ that the project is completed on time.", "id": "ensuring" },
  { "en": "The author draws an ___ between the human brain and a computer.", "id": "analogy" },
  { "en": "The hotel is ___ located near the city center.", "id": "conveniently" },
  { "en": "It is ___ to assume that he will agree to the proposal.", "id": "reasonable" },
  { "en": "The course provides students with the ___ skills for a career in marketing.", "id": "necessary" },
  { "en": "The mountain's peak is ___ visible from the town.", "id": "rarely" },
  { "en": "The new law has been the ___ of much debate.", "id": "subject" },
  { "en": "The two designs are ___ identical, but there are minor differences.", "id": "virtually" },
  { "en": "The experiment's results were ___ by external factors.", "id": "affected" },
  { "en": "There is ___ evidence to support his claim.", "id": "ample" },
  { "en": "The library is ___ to university students and staff only.", "id": "accessible" },
  { "en": "The government has put a greater ___ on education.", "id": "emphasis" },
  { "en": "The treaty was a ___ achievement in international diplomacy.", "id": "landmark" },
  { "en": "She has always ___ to be a successful writer.", "id": "aspired" },
  { "en": "His decision to resign was ___, to say the least.", "id": "unexpected" },
  { "en": "The company must ___ to the new safety standards.", "id": "adhere" },
  { "en": "The information is ___ from a reliable source.", "id": "derived" },
  { "en": "He spoke ___ on a wide range of topics.", "id": "eloquently" },
  { "en": "The agreement is ___ on both parties.", "id": "binding" },
  { "en": "They are working to find a ___ solution to the conflict.", "id": "peaceful" },
  { "en": "It was the manager ___ approved the final budget.", "id": "that" },
  { "en": "___ I need right now is a long vacation.", "id": "What" },
  { "en": "It is in this city ___ the treaty was signed.", "id": "that" },
  { "en": "___ surprised the investors was the company's sudden bankruptcy.", "id": "What" },
  { "en": "It was not until yesterday ___ I received the news.", "id": "that" },
  { "en": "I can't find my keys ___.", "id": "anywhere" },
  { "en": "He had ___ money left after his trip.", "id": "no" },
  { "en": "It is not ___ for students to feel nervous before an exam.", "id": "uncommon" },
  { "en": "She could ___ believe what she was hearing.", "id": "hardly" },
  { "en": "There isn't ___ reason to doubt his story.", "id": "any" },
  { "en": "I wish I ___ more about the topic before the discussion.", "id": "had known" },
  { "en": "He wishes he ___ taller.", "id": "were" },
  { "en": "If only I ___ to her advice.", "id": "had listened" },
  { "en": "She wishes she ___ travel more often.", "id": "could" },
  { "en": "They wish the rain ___ stop so they can go outside.", "id": "would" },
  { "en": "I saw the man ___ out of the building.", "id": "run" },
  { "en": "We watched the sun ___ over the horizon.", "id": "setting" },
  { "en": "Did you hear the baby ___ last night?", "id": "cry" },
  { "en": "I can feel the ground ___ beneath my feet.", "id": "shaking" },
  { "en": "The main cause of these accidents ___ driver error.", "id": "is" },
  { "en": "The quality of the products ___ declined recently.", "id": "has" },
  { "en": "A box of old letters ___ found in the attic.", "id": "was" },
  { "en": "The list of candidates ___ not been finalized yet.", "id": "has" },
  { "en": "The CEO, together with the board members, ___ to make a statement.", "id": "is" },
  { "en": "Some of the information ___ out of date.", "id": "is" },
  { "en": "Most of the students ___ passed the exam.", "id": "have" },
  { "en": "None of the equipment ___ working properly.", "id": "is" },
  { "en": "All of the cake ___ been eaten.", "id": "has" },
  { "en": "Half of the apples ___ rotten.", "id": "are" },
  { "en": "I have finished this book; can I borrow ___ one?", "id": "another" },
  { "en": "Some people like to travel; ___ prefer to stay at home.", "id": "others" },
  { "en": "This shoe fits perfectly, but ___ one is too small.", "id": "the other" },
  { "en": "She works much harder than ___.", "id": "he does" },
  { "en": "While ___ on vacation, he rarely checked his email.", "id": "he was" },
  { "en": "The government is taking steps to ___ the problem of unemployment.", "id": "tackle" },
  { "en": "His research provided ___ evidence for his theory.", "id": "empirical" },
  { "en": "The two countries have a ___ relationship, often disagreeing on policy.", "id": "contentious" },
  { "en": "The transition to a new system has not been entirely ___.", "id": "seamless" },
  { "en": "He has a ___ ability to simplify complex ideas.", "id": "unique" },
  { "en": "The company is trying to ___ its public image.", "id": "enhance" },
  { "en": "The new law places several ___ on small businesses.", "id": "constraints" },
  { "en": "The lecture was ___ to a wide audience.", "id": "geared" },
  { "en": "It is ___ that you read the contract carefully before signing.", "id": "imperative" },
  { "en": "The project was a ___ failure, costing the company millions.", "id": "colossal" },
  { "en": "The report highlights the ___ need for reform.", "id": "urgent" },
  { "en": "The artist's early work was heavily ___ by cubism.", "id": "influenced" },
  { "en": "We must use our natural resources ___.", "id": "wisely" },
  { "en": "The defendant's alibi was ___ by two witnesses.", "id": "corroborated" },
  { "en": "The city is a ___ pot of different cultures.", "id": "melting" },
  { "en": "The internet has ___ the way we communicate.", "id": "revolutionized" },
  { "en": "The company is facing ___ competition from foreign firms.", "id": "fierce" },
  { "en": "He was forced to ___ his position as chairman.", "id": "relinquish" },
  { "en": "His health has shown a ___ improvement.", "id": "marked" },
  { "en": "The documentary offers a ___ glimpse into the lives of wolves.", "id": "rare" },
  { "en": "The new software is not backwards ___ with older versions.", "id": "compatible" },
  { "en": "The organization is ___ to protecting the environment.", "id": "dedicated" },
  { "en": "He is a ___ of the company's new environmental policy.", "id": "proponent" },
  { "en": "The rules are ___ and must be followed by everyone.", "id": "explicit" },
  { "en": "She has ___ a great deal of knowledge on the subject.", "id": "accumulated" },
  { "en": "The economic forecast for next year is ___.", "id": "bleak" },
  { "en": "The two events happened ___.", "id": "simultaneously" },
  { "en": "The project is still in its ___ stages.", "id": "initial" },
  { "en": "The company's success is ___ on its ability to innovate.", "id": "dependent" },
  { "en": "It is ___ to drink plenty of water in hot weather.", "id": "advisable" },
  { "en": "The treaty aims to ___ peace between the two nations.", "id": "ensure" },
  { "en": "The witness gave a ___ description of the suspect.", "id": "vague" },
  { "en": "His ___ to the team has been invaluable.", "id": "contribution" },
  { "en": "The museum's most prized ___ is a painting by Van Gogh.", "id": "possession" },
  { "en": "The effects of the drought will be ___-reaching.", "id": "far" },
  { "en": "The results of the study were not statistically ___.", "id": "significant" },
  { "en": "The students were asked to give a ___ presentation.", "id": "brief" },
  { "en": "His leadership was ___ in resolving the conflict.", "id": "instrumental" },
  { "en": "The population of the city has grown at an ___ rate.", "id": "unprecedented" },
  { "en": "The company has a ___ for excellent customer service.", "id": "reputation" },
  { "en": "The details of the plan are still ___.", "id": "tentative" },
  { "en": "The new ___ are intended to improve safety in the workplace.", "id": "regulations" },
  { "en": "The forest is home to a ___ array of wildlife.", "id": "diverse" },
  { "en": "His argument is based on a ___ premise.", "id": "flawed" },
  { "en": "The decision was made by ___ among the committee members.", "id": "consensus" },
  { "en": "The athlete is at the ___ of her career.", "id": "peak" },
  { "en": "The company is looking for ways to ___ its costs.", "id": "minimize" },
  { "en": "The archaeological dig yielded some ___ artifacts.", "id": "priceless" },
  { "en": "The evidence is ___ and does not point to a single conclusion.", "id": "ambiguous" },
  { "en": "He took a ___ approach to solving the problem.", "id": "systematic" },
  { "en": "There has been a ___ shift in public opinion.", "id": "noticeable" },
  { "en": "The negotiations reached a ___ and could not proceed.", "id": "stalemate" },
  { "en": "The two systems are essentially the same, ___ with minor variations.", "id": "albeit" },
  { "en": "The book provides a ___ overview of the country's history.", "id": "concise" },
  { "en": "The city has a ___ public transportation system.", "id": "reliable" },
  { "en": "The manager is ___ for the day-to-day operations of the store.", "id": "responsible" },
  { "en": "The launch of the new product was ___ with a major marketing campaign.", "id": "coordinated" },
  { "en": "The company is vulnerable ___ a hostile takeover.", "id": "to" },
  { "en": "The investigation ___ a number of irregularities.", "id": "uncovered" },
  { "en": "The long-term ___ of the new policy are still unknown.", "id": "ramifications" },
  { "en": "I need to ___ my car serviced before the long trip.", "id": "have" },
  { "en": "She is going to get her portrait ___ by a famous artist.", "id": "painted" },
  { "en": "The company had all its computers ___ with the new software.", "id": "updated" },
  { "en": "He couldn't fix the leak himself, so he ___ a plumber do it.", "id": "had" },
  { "en": "We must get this document ___ by a notary public.", "id": "signed" },
  { "en": "When I was a child, I ___ play in this park every day.", "id": "used to" },
  { "en": "He is not ___ the cold weather yet; he just moved from a tropical country.", "id": "used to" },
  { "en": "It took me a while to get ___ driving on the left side of the road.", "id": "used to" },
  { "en": "Did you ___ live in this neighborhood?", "id": "use to" },
  { "en": "She is a flight attendant, so she is used to ___.", "id": "flying" },
  { "en": "The movie was ___ good, but it wasn't the best I've ever seen.", "id": "quite" },
  { "en": "I would ___ not go out tonight; I'm very tired.", "id": "rather" },
  { "en": "The temperature was ___ cold for this time of year.", "id": "extremely" },
  { "en": "He speaks so fast that it's ___ impossible to understand him.", "id": "almost" },
  { "en": "The exam was ___ difficult, but most students managed to pass.", "id": "fairly" },
  { "en": "The lights are on, so they ___ be home.", "id": "must" },
  { "en": "He didn't answer his phone. He ___ have been sleeping.", "id": "might" },
  { "en": "She looks very unhappy. She ___ have heard some bad news.", "id": "must" },
  { "en": "He just ate a huge meal. He ___ be hungry again already.", "id": "can't" },
  { "en": "The package arrived this morning, but I didn't order anything. It ___ be for my roommate.", "id": "must" },
  { "en": "In ___ to being a great musician, he is also a talented painter.", "id": "addition" },
  { "en": "He was criticized ___ not finishing the project on time.", "id": "for" },
  { "en": "The new law aims at ___ pollution in the city.", "id": "reducing" },
  { "en": "She insisted ___ paying for everyone's dinner.", "id": "on" },
  { "en": "There is no point ___ about something you cannot change.", "id": "in worrying" },
  { "en": "Could you please ___ the music down? It's too loud.", "id": "turn" },
  { "en": "I need to ___ this form out before I submit my application.", "id": "fill" },
  { "en": "She had to ___ her children up from school at 3 PM.", "id": "pick" },
  { "en": "Before you leave, please make sure to ___ all the lights.", "id": "turn off" },
  { "en": "He ___ the meeting off until the following week.", "id": "put" },
  { "en": "The Amazon River flows through ___ South America.", "id": "(no article)" },
  { "en": "They went hiking in ___ Rocky Mountains last summer.", "id": "the" },
  { "en": "The ship sailed across ___ Pacific Ocean.", "id": "the" },
  { "en": "___ Sahara Desert is the largest hot desert in the world.", "id": "The" },
  { "en": "Mount Everest, ___ highest mountain in the world, is in the Himalayas.", "id": "the" },
  { "en": "The government implemented new policies to ___ poverty.", "id": "alleviate" },
  { "en": "The company plans to ___ its workforce by hiring 100 new employees.", "id": "augment" },
  { "en": "There was a significant ___ between the two reports.", "id": "discrepancy" },
  { "en": "The committee is looking for a ___ solution to the budget crisis.", "id": "feasible" },
  { "en": "The sudden drop in sales was an ___ for the company's financial problems.", "id": "indicator" },
  { "en": "The new CEO's main ___ is to improve profitability.", "id": "priority" },
  { "en": "The witness's statement was ___ to the investigation.", "id": "crucial" },
  { "en": "The two companies are working in ___ to develop a new product.", "id": "collaboration" },
  { "en": "The ancient manuscript is ___; it's very delicate.", "id": "fragile" },
  { "en": "The project requires a ___ amount of time and resources.", "id": "considerable" },
  { "en": "The new drug is a ___ treatment for the disease.", "id": "potent" },
  { "en": "The final chapter ___ the main points of the book.", "id": "summarizes" },
  { "en": "The company has an ___ to provide a safe working environment.", "id": "obligation" },
  { "en": "The results of the experiment were ___ with our initial hypothesis.", "id": "inconsistent" },
  { "en": "His argument is logical and ___.", "id": "coherent" },
  { "en": "The goal is to ___ the gap between the rich and the poor.", "id": "bridge" },
  { "en": "The city is facing a severe water ___.", "id": "shortage" },
  { "en": "The two designs ___ in several important ways.", "id": "differ" },
  { "en": "She has a ___ knowledge of art history.", "id": "profound" },
  { "en": "The government is expected to ___ new guidelines soon.", "id": "issue" },
  { "en": "The old building is a fire ___.", "id": "hazard" },
  { "en": "The new discovery has ___ for the future of space exploration.", "id": "implications" },
  { "en": "It is ___ that he will win the election.", "id": "probable" },
  { "en": "The company is conducting a ___ review of its policies.", "id": "comprehensive" },
  { "en": "The defendant was ___ to three years in prison.", "id": "sentenced" },
  { "en": "The artist's work is a ___ of her personal experiences.", "id": "reflection" },
  { "en": "The system is designed to be highly ___.", "id": "efficient" },
  { "en": "The two countries have ___ diplomatic relations for decades.", "id": "maintained" },
  { "en": "The study's results were ___ by a team of independent researchers.", "id": "verified" },
  { "en": "He provided a ___ explanation for his absence.", "id": "plausible" },
  { "en": "The city has a ___ cultural heritage.", "id": "rich" },
  { "en": "The new manager plans to ___ several changes.", "id": "introduce" },
  { "en": "The success of the project is ___ to good planning.", "id": "attributable" },
  { "en": "The museum's collection ___ several famous paintings.", "id": "includes" },
  { "en": "The company had to ___ a number of employees during the recession.", "id": "lay off" },
  { "en": "The two languages have some ___ similarities.", "id": "striking" },
  { "en": "The program aims to ___ students for the workforce.", "id": "prepare" },
  { "en": "The decision was made ___ with the company's policy.", "id": "in accordance" },
  { "en": "The research focuses ___ on the effects of social media.", "id": "primarily" },
  { "en": "The author's writing style is very ___.", "id": "distinctive" },
  { "en": "The two sides have reached a ___ agreement.", "id": "tentative" },
  { "en": "The company is making a ___ effort to reduce waste.", "id": "concerted" },
  { "en": "The new evidence ___ him from all suspicion.", "id": "absolved" },
  { "en": "The government failed to ___ the needs of the homeless.", "id": "address" },
  { "en": "Her work is a ___ source of inspiration for many young artists.", "id": "constant" },
  { "en": "He is ___ to making a decision until he has all the facts.", "id": "reluctant" },
  { "en": "The two nations have a ___ agreement to share intelligence.", "id": "reciprocal" },
  { "en": "The project was a ___ success.", "id": "resounding" },
  { "en": "The law is not ___ in this particular case.", "id": "applicable" },
  { "en": "The company is under ___ to improve its financial performance.", "id": "pressure" },
  { "en": "The evidence against him was ___.", "id": "overwhelming" },
  { "en": "The doctor recommended a ___ change in his diet.", "id": "gradual" },
  { "en": "The company has ___ a new strategy for growth.", "id": "adopted" },
  { "en": "Her knowledge of the subject is ___.", "id": "extensive" },
  { "en": "He is ___ as the leading authority on the topic.", "id": "recognized" },
  { "en": "The two companies ___ to form a new, larger corporation.", "id": "merged" },
  { "en": "The city has a ___ of parks and green spaces.", "id": "network" },
  { "en": "It is ___ to check your work for errors before submitting it.", "id": "prudent" },
  { "en": "The novel gives a ___ portrayal of life in the 19th century.", "id": "vivid" },
  { "en": "The company had to ___ production due to a lack of raw materials.", "id": "suspend" },
  { "en": "The new system is ___ to be more user-friendly.", "id": "intended" },
  { "en": "___ his help, we would not have finished the project on time.", "id": "Without" },
  { "en": "But ___ your timely intervention, the situation would have been much worse.", "id": "for" },
  { "en": "The loan will be approved ___ that you provide proof of income.", "id": "on the condition" },
  { "en": "In case ___ an emergency, please use the nearest exit.", "id": "of" },
  { "en": "___ I known about the traffic, I would have left earlier.", "id": "Had" },
  { "en": "The weather was ___ cold that the river froze solid.", "id": "so" },
  { "en": "It was ___ a difficult test that many students failed.", "id": "such" },
  { "en": "He spoke ___ quickly that I could not understand him.", "id": "so" },
  { "en": "She has ___ a beautiful voice that she should become a professional singer.", "id": "such" },
  { "en": "There were ___ many people at the concert that we couldn't find a seat.", "id": "so" },
  { "en": "The person to ___ I spoke was very helpful.", "id": "whom" },
  { "en": "This is the project on ___ we have been working for months.", "id": "which" },
  { "en": "The manager, for ___ we have the utmost respect, is retiring next year.", "id": "whom" },
  { "en": "The theory upon ___ his research is based is quite complex.", "id": "which" },
  { "en": "The period during ___ the empire flourished was known as the Golden Age.", "id": "which" },
  { "en": "We are all eager ___ the results of the election.", "id": "to see" },
  { "en": "He is very good ___ solving complex puzzles.", "id": "at" },
  { "en": "It is likely ___ rain later this afternoon.", "id": "to" },
  { "en": "I am tired ___ the same thing over and over again.", "id": "of doing" },
  { "en": "She was hesitant ___ her opinion.", "id": "to share" },
  { "en": "On the hill ___ a beautiful old castle.", "id": "stands" },
  { "en": "Among the documents ___ a letter from the president.", "id": "was" },
  { "en": "Beyond the mountains ___ a vast desert.", "id": "lies" },
  { "en": "Never before ___ I witnessed such a spectacle.", "id": "have" },
  { "en": "The prize was divided between my brother and ___.", "id": "me" },
  { "en": "My colleague and ___ will be presenting the report.", "id": "I" },
  { "en": "She is at least as qualified as ___.", "id": "he is" },
  { "en": "The manager gave the assignment to ___ and her partner.", "id": "him" },
  { "en": "It was ___ who called the police.", "id": "she" },
  { "en": "The team practiced hard; ___, they won the championship.", "id": "consequently" },
  { "en": "He is an expert in physics; ___, he is a talented musician.", "id": "moreover" },
  { "en": "The new model is more efficient. ___, it is also more expensive.", "id": "However" },
  { "en": "The kitchen was small; ___, the living room was quite spacious.", "id": "in contrast" },
  { "en": "He studied all night; ___, he felt prepared for the exam.", "id": "thus" },
  { "en": "The ___ of the new policy will be evaluated after six months.", "id": "efficacy" },
  { "en": "The professor's explanation helped to ___ the complex theory.", "id": "elucidate" },
  { "en": "His argument was full of logical ___.", "id": "fallacies" },
  { "en": "The two cultures share an ___ respect for their elders.", "id": "inherent" },
  { "en": "The government's new approach represents a ___ shift in policy.", "id": "paradigm" },
  { "en": "The company has a ___ on a new recycling program.", "id": "embarked" },
  { "en": "The symptoms of the disease can be quite ___.", "id": "subtle" },
  { "en": "The peace talks were a ___ effort to end the long-standing conflict.", "id": "deliberate" },
  { "en": "The lawyer presented ___ evidence to support her client's case.", "id": "compelling" },
  { "en": "The ruins of the ancient city are ___ of its former glory.", "id": "reminiscent" },
  { "en": "The organization is ___ to the welfare of animals.", "id": "committed" },
  { "en": "The study was ___ in its approach, covering all possible angles.", "id": "exhaustive" },
  { "en": "The CEO gave a ___ speech about the company's future.", "id": "rousing" },
  { "en": "His decision to leave was ___ by a desire for new challenges.", "id": "motivated" },
  { "en": "The government must take ___ measures to control inflation.", "id": "decisive" },
  { "en": "The discovery of the new species was purely ___.", "id": "fortuitous" },
  { "en": "The two accounts of the event are not mutually ___.", "id": "exclusive" },
  { "en": "The professor is a ___ expert on ancient civilizations.", "id": "preeminent" },
  { "en": "The new evidence ___ the need for a new investigation.", "id": "underscores" },
  { "en": "The manual provides ___ instructions for assembling the furniture.", "id": "step-by-step" },
  { "en": "The company's business practices are ethically ___.", "id": "questionable" },
  { "en": "The treaty was a ___ of years of negotiation.", "id": "culmination" },
  { "en": "His writing style is ___ and often difficult to understand.", "id": "convoluted" },
  { "en": "The organization provides a ___ for professionals to network and share ideas.", "id": "forum" },
  { "en": "The new vaccine is expected to ___ immunity against the virus.", "id": "confer" },
  { "en": "The two politicians have ___ opposing views on the issue.", "id": "diametrically" },
  { "en": "The ___ of the problem lies in a lack of funding.", "id": "crux" },
  { "en": "The new system is ___ superior to the old one.", "id": "demonstrably" },
  { "en": "The army's ___ of the city lasted for several months.", "id": "siege" },
  { "en": "His speech was met with a ___ of applause.", "id": "torrent" },
  { "en": "The evidence is ___ and open to interpretation.", "id": "subjective" },
  { "en": "The company tries to ___ a positive work environment.", "id": "cultivate" },
  { "en": "His theories were once considered radical but are now ___.", "id": "mainstream" },
  { "en": "The witness gave a ___ account of the events leading up to the accident.", "id": "chronological" },
  { "en": "The plan is still in its ___ stages of development.", "id": "preliminary" },
  { "en": "He is known for his ___ attention to detail.", "id": "meticulous" },
  { "en": "The project was ___ with difficulties from the start.", "id": "fraught" },
  { "en": "Her ___ as a skilled negotiator is well-deserved.", "id": "reputation" },
  { "en": "The company has a ___ of not paying its suppliers on time.", "id": "track record" },
  { "en": "The economic ___ for the coming year is uncertain.", "id": "outlook" },
  { "en": "The two sides are unwilling to ___ on the key issues.", "id": "compromise" },
  { "en": "The building is a prime ___ of modern architecture.", "id": "example" },
  { "en": "The committee has a ___ from the board to investigate the matter.", "id": "mandate" },
  { "en": "The findings of the report are ___ and require more data.", "id": "provisional" },
  { "en": "The defendant's story was not ___.", "id": "credible" },
  { "en": "The two versions of the story ___ significantly.", "id": "diverge" },
  { "en": "His knowledge of the subject is ___, but not very deep.", "id": "broad" },
  { "en": "The company's failure was ___ to poor marketing.", "id": "attributed" },
  { "en": "The software has a very ___ user interface.", "id": "intuitive" },
  { "en": "The city is ___ for its vibrant nightlife.", "id": "noted" },
  { "en": "The long-term ___ of climate change are a major concern.", "id": "consequences" },
  { "en": "She has a ___ for saying the right thing at the right time.", "id": "knack" },
  { "en": "The debate was ___ and failed to resolve the key issues.", "id": "acrimonious" },
  { "en": "The new policy is designed to ___ economic growth.", "id": "foster" },
  { "en": "The student's essay was ___ and well-argued.", "id": "articulate" },
  { "en": "The two nations have agreed to ___ hostilities.", "id": "cease" },
  { "en": "The company is at the ___ of technological innovation.", "id": "forefront" },
  { "en": "The evidence is ___ to support a conviction.", "id": "inadequate" },
  { "en": "The new law gives the police ___ powers.", "id": "sweeping" },
  { "en": "No sooner had the president started speaking ___ the power went out.", "id": "than" },
  { "en": "Hardly had she arrived at the station ___ the train departed.", "id": "when" },
  { "en": "Scarcely had we sat down for dinner ___ the doorbell rang.", "id": "when" },
  { "en": "No sooner ___ the team score a goal than the crowd erupted in cheers.", "id": "did" },
  { "en": "Hardly ___ the movie begun when my phone started ringing.", "id": "had" },
  { "en": "He talks as if he ___ the expert on everything.", "id": "were" },
  { "en": "She acted as though she ___ me before, but I'm sure we've never met.", "id": "had met" },
  { "en": "The child looked at the broken toy as if the world ___ to an end.", "id": "had come" },
  { "en": "He spends money as though he ___ a millionaire.", "id": "were" },
  { "en": "It looks as if it ___ going to rain.", "id": "is" },
  { "en": "The coffee is not hot ___ for me.", "id": "enough" },
  { "en": "We don't have ___ to complete the project.", "id": "enough time" },
  { "en": "Is he old ___ to vote in the election?", "id": "enough" },
  { "en": "There isn't ___ for everyone to have a seat.", "id": "enough room" },
  { "en": "He didn't run quickly ___ to win the race.", "id": "enough" },
  { "en": "I am not sure ___ I can attend the meeting.", "id": "whether" },
  { "en": "The decision depends on ___ we get the funding.", "id": "whether" },
  { "en": "The main question is ___ to proceed with the plan or not.", "id": "whether" },
  { "en": "He asked ___ I wanted to join them for lunch.", "id": "if" },
  { "en": "I doubt ___ he will finish on time.", "id": "whether" },
  { "en": "This form needs ___ by tomorrow morning.", "id": "to be signed" },
  { "en": "He resents ___ what to do by his younger colleagues.", "id": "being told" },
  { "en": "The new software is expected ___ on all computers by next week.", "id": "to be installed" },
  { "en": "She avoids ___ for her political opinions.", "id": "being criticized" },
  { "en": "The house requires ___ before we can move in.", "id": "painting" },
  { "en": "Her responsibilities include managing the team, preparing the budget, and ___ with clients.", "id": "liaising" },
  { "en": "He is a man of great intelligence, courage, and ___.", "id": "integrity" },
  { "en": "The report criticized the company for its poor safety record, its lack of transparency, and ___ it treated its employees.", "id": "the way" },
  { "en": "To succeed in this field, one must be dedicated, persistent, and ___.", "id": "able to learn quickly" },
  { "en": "The company's goals are not only to maximize profit but also ___ a positive impact on the community.", "id": "to create" },
  { "en": "Smartphones have become ___ in modern society.", "id": "ubiquitous" },
  { "en": "Youth is ___; it does not last forever.", "id": "ephemeral" },
  { "en": "We need a ___ solution to this problem, not just a theoretical one.", "id": "pragmatic" },
  { "en": "___, he was a supporter of the new policy, but in reality, he opposed it.", "id": "Ostensibly" },
  { "en": "Misinformation tends to ___ quickly on social media.", "id": "proliferate" },
  { "en": "The two articles ___ the issue from different perspectives.", "id": "approach" },
  { "en": "The research aims to ___ the relationship between diet and health.", "id": "investigate" },
  { "en": "There is a ___ difference between the two concepts.", "id": "fundamental" },
  { "en": "The government must ___ resources effectively to meet the needs of the people.", "id": "allocate" },
  { "en": "The new law is intended to ___ consumer rights.", "id": "protect" },
  { "en": "His speech had a ___ effect on the audience.", "id": "profound" },
  { "en": "The company has a ___ process for handling customer complaints.", "id": "standardized" },
  { "en": "The committee reached a ___ decision to approve the project.", "id": "unanimous" },
  { "en": "The historical ___ of the event is still debated by scholars.", "id": "significance" },
  { "en": "The country's economy is heavily ___ on tourism.", "id": "reliant" },
  { "en": "The new evidence ___ the defendant's claim of innocence.", "id": "reinforces" },
  { "en": "The plan is ___ because it requires too many resources.", "id": "unfeasible" },
  { "en": "The two nations have a ___ agreement to support each other in times of crisis.", "id": "mutual" },
  { "en": "The doctor gave a ___ diagnosis after examining the patient.", "id": "preliminary" },
  { "en": "The city has a ___ problem with traffic congestion.", "id": "chronic" },
  { "en": "The ___ of the problem is more complex than it first appears.", "id": "nature" },
  { "en": "The old bridge is no longer structurally ___.", "id": "sound" },
  { "en": "The professor tried to ___ a sense of curiosity in her students.", "id": "instill" },
  { "en": "The company is making a ___ shift towards renewable energy.", "id": "gradual" },
  { "en": "The book provides a ___ critique of modern society.", "id": "scathing" },
  { "en": "His promotion was the ___ of years of hard work.", "id": "culmination" },
  { "en": "The new manager is expected to ___ the company's new policies.", "id": "implement" },
  { "en": "The witness's testimony was found to be ___.", "id": "unreliable" },
  { "en": "The two countries have a long history of ___ cooperation.", "id": "bilateral" },
  { "en": "The government is facing ___ pressure to lower taxes.", "id": "immense" },
  { "en": "The new drug is a ___ breakthrough in cancer treatment.", "id": "potential" },
  { "en": "The company has a ___ process for quality control.", "id": "rigorous" },
  { "en": "The team's ___ was high after winning the championship.", "id": "morale" },
  { "en": "The defendant was ___ of the crime due to a lack of evidence.", "id": "acquitted" },
  { "en": "The artist's work is a ___ to the beauty of nature.", "id": "tribute" },
  { "en": "The new regulations will have ___-reaching effects on the industry.", "id": "far" },
  { "en": "The company's profits ___ all expectations last year.", "id": "exceeded" },
  { "en": "The new evidence ___ his earlier testimony.", "id": "contradicts" },
  { "en": "The two systems are not ___, so they cannot be used together.", "id": "compatible" },
  { "en": "The negotiations reached an ___ and could not proceed.", "id": "impasse" },
  { "en": "The government is trying to ___ the flow of illegal drugs.", "id": "curb" },
  { "en": "His decision was ___ by logic and reason.", "id": "guided" },
  { "en": "The company is known for its ___ commitment to sustainability.", "id": "unwavering" },
  { "en": "The book gives a ___ account of the war.", "id": "harrowing" },
  { "en": "He is a ___ figure in the environmental movement.", "id": "prominent" },
  { "en": "The old traditions are gradually ___ away.", "id": "fading" },
  { "en": "The meeting was ___ to discuss the quarterly report.", "id": "convened" },
  { "en": "His theory, ___ simple, is actually quite complex.", "id": "deceptively" },
  { "en": "The two projects will be managed ___ until a decision is made.", "id": "concurrently" },
  { "en": "The museum's collection is ___ and covers many centuries.", "id": "vast" },
  { "en": "The new discovery has ___ a lot of interest among scientists.", "id": "generated" },
  { "en": "She is an ___ for human rights.", "id": "advocate" },
  { "en": "The company had to ___ its strategy in response to market changes.", "id": "modify" },
  { "en": "The city has a ___ of being a safe place to live.", "id": "reputation" },
  { "en": "The two ideas are not ___; in fact, they support each other.", "id": "contradictory" },
  { "en": "The project is a ___ between the university and a private company.", "id": "collaboration" },
  { "en": "The evidence provided was ___ to reach a conclusion.", "id": "insufficient" },
  { "en": "The new policy will be ___ from the first of next month.", "id": "effective" },
  { "en": "He has a ___ for remembering names and faces.", "id": "talent" },
  { "en": "The study's findings are ___ with previous research.", "id": "consistent" },
  { "en": "The company offers ___ benefits to its employees.", "id": "generous" },
  { "en": "The issue is ___ to the main topic of discussion.", "id": "peripheral" },
  { "en": "The organization is ___ by a board of directors.", "id": "governed" },
  { "en": "The witness's memory of the event was ___.", "id": "hazy" },
  { "en": "The new plan is a ___ improvement over the old one.", "id": "demonstrable" },
  { "en": "His ___ to the cause is unquestionable.", "id": "dedication" },
  { "en": "The report provides a ___ analysis of the market trends.", "id": "thorough" },
  { "en": "The new discovery could be the ___ to solving the mystery.", "id": "key" },
  { "en": "The company has ___ its position as a market leader.", "id": "consolidated" },
  { "en": "It's late. It's high time we ___ home.", "id": "went" },
  { "en": "You have been procrastinating for weeks. It's time you ___ your project.", "id": "started" },
  { "en": "It's about time the government ___ something about this problem.", "id": "did" },
  { "en": "I think it's time for us ___ the discussion.", "id": "to start" },
  { "en": "It's time ___ a decision.", "id": "to make" },
  { "en": "I would rather ___ at home than go to the movies tonight.", "id": "stay" },
  { "en": "The deadline is tomorrow. You ___ finish that report now.", "id": "had better" },
  { "en": "She would rather her son ___ a doctor than a lawyer.", "id": "became" },
  { "en": "He'd rather ___ than admit he was wrong.", "id": "resign" },
  { "en": "He'd better ___ late again, or he will be in serious trouble.", "id": "not be" },
  { "en": "Please remember ___ the door when you leave.", "id": "to lock" },
  { "en": "He stopped ___ because the doctor advised him to.", "id": "smoking" },
  { "en": "I forgot ___ him about the meeting, so he didn't attend.", "id": "to tell" },
  { "en": "I'll never forget ___ the Grand Canyon for the first time.", "id": "seeing" },
  { "en": "She tried ___ the heavy box, but it was too heavy for her.", "id": "to lift" },
  { "en": "___ students find the final exam to be the most difficult.", "id": "Most" },
  { "en": "Most of ___ in my class are from other countries.", "id": "the students" },
  { "en": "Some of ___ you gave me was incorrect.", "id": "the information" },
  { "en": "___ people enjoy listening to music.", "id": "Most" },
  { "en": "Many of ___ in this parking lot are electric.", "id": "the cars" },
  { "en": "She is very proficient ___ several programming languages.", "id": "in" },
  { "en": "The final grade is contingent ___ your exam results.", "id": "on" },
  { "en": "He was exonerated ___ all charges against him.", "id": "from" },
  { "en": "The new design is a departure ___ the company's traditional style.", "id": "from" },
  { "en": "The team is composed ___ of ten players.", "id": "of" },
  { "en": "He studied diligently ___ he should fail the exam.", "id": "lest" },
  { "en": "The policy is a good one ___ it is implemented fairly.", "id": "provided that" },
  { "en": "___ he is an expert, his opinion is highly valued.", "id": "Inasmuch as" },
  { "en": "He will be promoted ___ he meets the sales targets.", "id": "as long as" },
  { "en": "They were found guilty ___ they had a strong alibi.", "id": "even though" },
  { "en": "The value of friendship is ___; it comes from within and cannot be bought.", "id": "intrinsic" },
  { "en": "The decision to close the factory was completely ___, with no logical reason given.", "id": "arbitrary" },
  { "en": "The evidence he presented was later found to be ___.", "id": "spurious" },
  { "en": "His claim to the throne was rather ___, based on a distant ancestor.", "id": "tenuous" },
  { "en": "The report tries to ___ the city's modern architecture with its ancient ruins.", "id": "juxtapose" },
  { "en": "His ___ for the job was clear from his enthusiastic interview.", "id": "zeal" },
  { "en": "The politician's speech was full of empty ___.", "id": "rhetoric" },
  { "en": "The company had to ___ its operations during the economic crisis.", "id": "curtail" },
  { "en": "The two theories are not contradictory; in fact, they are ___.", "id": "complementary" },
  { "en": "The documentary was a ___ indictment of the government's policies.", "id": "damning" },
  { "en": "The professor's lectures were often ___ and difficult for the students to follow.", "id": "esoteric" },
  { "en": "There was a ___ agreement among the team members to support their leader.", "id": "tacit" },
  { "en": "He gave the report a ___ glance before signing it.", "id": "perfunctory" },
  { "en": "Her ___ nature made her an excellent diplomat.", "id": "tactful" },
  { "en": "The rise of remote work has ___ the need for large office spaces.", "id": "obviated" },
  { "en": "The new policy has been met with ___ from the public.", "id": "skepticism" },
  { "en": "The city's ___ population has grown rapidly in recent years.", "id": "indigenous" },
  { "en": "The artist is known for his ___ use of bold colors.", "id": "audacious" },
  { "en": "The company is trying to ___ its brand identity.", "id": "redefine" },
  { "en": "The evidence was ___ and did not support his claim.", "id": "flimsy" },
  { "en": "The manager's ___ leadership style was very effective.", "id": "hands-on" },
  { "en": "The new law is an ___ on personal freedom.", "id": "infringement" },
  { "en": "The company is facing a ___ of lawsuits.", "id": "plethora" },
  { "en": "He has a ___ ability to predict market trends.", "id": "uncanny" },
  { "en": "The two versions of the document need to be ___.", "id": "reconciled" },
  { "en": "The project was a ___ failure, despite all the planning.", "id": "dismal" },
  { "en": "The new evidence may ___ the defendant.", "id": "exonerate" },
  { "en": "His ___ to change is a major obstacle to progress.", "id": "resistance" },
  { "en": "The two countries have a ___ trade agreement.", "id": "long-standing" },
  { "en": "The organization is ___ with finding a cure for the disease.", "id": "tasked" },
  { "en": "The company's code of conduct is ___ and applies to all employees.", "id": "non-negotiable" },
  { "en": "The new discovery has ___ our understanding of the universe.", "id": "transformed" },
  { "en": "The project is financially ___ and likely to succeed.", "id": "viable" },
  { "en": "The two sides are in a ___ over the terms of the contract.", "id": "dispute" },
  { "en": "The manager's ___ approval is required for all expenses.", "id": "prior" },
  { "en": "The city is a ___ hub for trade and commerce.", "id": "major" },
  { "en": "The new evidence ___ the case against him.", "id": "strengthens" },
  { "en": "His work is ___ of a true professional.", "id": "characteristic" },
  { "en": "The company has a ___ approach to problem-solving.", "id": "proactive" },
  { "en": "The two leaders met to ___ their differences.", "id": "resolve" },
  { "en": "The study's results were ___ and require further analysis.", "id": "preliminary" },
  { "en": "The company is committed to ___ diversity in the workplace.", "id": "promoting" },
  { "en": "The defendant's alibi was ___ by several witnesses.", "id": "confirmed" },
  { "en": "The new regulations are ___ to all businesses, large and small.", "id": "applicable" },
  { "en": "The company is at a ___ in its development.", "id": "crossroads" },
  { "en": "The project must be completed within the ___ timeframe.", "id": "allotted" },
  { "en": "The company's success is ___ on innovation.", "id": "predicated" },
  { "en": "His ___ comments were not helpful to the discussion.", "id": "cynical" },
  { "en": "The organization works to ___ the effects of natural disasters.", "id": "mitigate" },
  { "en": "The ___ between the two candidates was televised nationally.", "id": "debate" },
  { "en": "The ___ of the matter is that we are running out of time.", "id": "reality" },
  { "en": "The company is taking ___ to improve its environmental record.", "id": "steps" },
  { "en": "His ___ to detail is remarkable.", "id": "attention" },
  { "en": "The two systems work in ___ to achieve the same goal.", "id": "tandem" },
  { "en": "The city's ___ plan includes new parks and public spaces.", "id": "urban" },
  { "en": "The project's success is ___ to the hard work of the entire team.", "id": "a testament" },
  { "en": "The new drug has shown ___ results in clinical trials.", "id": "promising" },
  { "en": "The company's financial situation is ___, but it is expected to improve.", "id": "precarious" },
  { "en": "He has a ___ for getting into trouble.", "id": "propensity" },
  { "en": "The new software is designed to ___ with existing systems.", "id": "integrate" },
  { "en": "The two versions of the story are ___ and cannot both be true.", "id": "incompatible" },
  { "en": "The company is ___ for its high ethical standards.", "id": "lauded" },
  { "en": "His ___ of the situation was completely wrong.", "id": "interpretation" },
  { "en": "The new system is ___ to be a major failure.", "id": "destined" },
  { "en": "The two countries have a ___ to avoid conflict.", "id": "shared interest" },
  { "en": "The project is a ___ of the company's commitment to innovation.", "id": "hallmark" },
  { "en": "The new regulations are ___ and will be difficult to implement.", "id": "unpopular" },
  { "en": "The two companies have a ___ to work together.", "id": "verbal agreement" },
  { "en": "His ___ is not relevant to the discussion.", "id": "personal opinion" },
  { "en": "___ by the complexity of the problem, the scientists requested more time.", "id": "Baffled" },
  { "en": "___ the necessary qualifications, he was not considered for the position.", "id": "Lacking" },
  { "en": "Once ___, this new technology will revolutionize the industry.", "id": "implemented" },
  { "en": "___ for many hours, the driver began to feel sleepy.", "id": "Having driven" },
  { "en": "When ___ about her future plans, the candidate gave a confident answer.", "id": "asked" },
  { "en": "My brother works in finance, and so ___ my sister.", "id": "does" },
  { "en": "She has never been to Asia, and ___ have I.", "id": "neither" },
  { "en": "They will not accept the proposal, and nor ___ the other committee members.", "id": "will" },
  { "en": "The previous report was inaccurate, and so ___ this one.", "id": "is" },
  { "en": "He can't speak German, and I can't ___.", "id": "either" },
  { "en": "A more experienced diplomat ___ have prevented the conflict.", "id": "might" },
  { "en": "I would have attended the conference, but I ___ a prior commitment.", "id": "had" },
  { "en": "He ran to the station; otherwise, he ___ the train.", "id": "would have missed" },
  { "en": "To hear him talk, you ___ think he was the CEO.", "id": "would" },
  { "en": "Another few minutes of delay, and the team ___ the match.", "id": "would have lost" },
  { "en": "Understanding the cultural nuances of a foreign country ___ essential for successful business negotiations.", "id": "is" },
  { "en": "Implementing such radical changes across all departments ___ a significant challenge.", "id": "presents" },
  { "en": "Having so many responsibilities at once ___ causing him a great deal of stress.", "id": "is" },
  { "en": "Analyzing the vast amounts of data from the experiment ___ several weeks.", "id": "took" },
  { "en": "Being able to speak multiple languages ___ a great advantage in the modern job market.", "id": "is" },
  { "en": "A number of important issues ___ discussed at the meeting.", "id": "were" },
  { "en": "The number of applicants for the program ___ exceeded our expectations.", "id": "has" },
  { "en": "Although the quality is high, a number of the products ___ minor defects.", "id": "have" },
  { "en": "The number of hours required to complete the project ___ underestimated.", "id": "was" },
  { "en": "The main office is located ___ 123 Main Street.", "id": "at" },
  { "en": "He lives ___ a small apartment on the third floor.", "id": "in" },
  { "en": "The store is closed ___ public holidays.", "id": "on" },
  { "en": "He was born ___ the 20th century.", "id": "in" },
  { "en": "___ the day, he works as a clerk, but at night he is a musician.", "id": "During" },
  { "en": "The tech startup is still in its ___ stage, but it shows great promise.", "id": "nascent" },
  { "en": "It is ___ upon the government to protect its citizens.", "id": "incumbent" },
  { "en": "The controversial remarks ___ an angry response from the public.", "id": "precipitated" },
  { "en": "The lack of rain ___ the effects of the drought.", "id": "exacerbated" },
  { "en": "He only has a ___ understanding of the subject; he needs to study more.", "id": "rudimentary" },
  { "en": "The speaker's argument was ___ and well-supported by evidence.", "id": "cogent" },
  { "en": "The treaty was intended to be binding, but it proved to be ___ in practice.", "id": "unenforceable" },
  { "en": "His ___ comments during the meeting were not helpful.", "id": "flippant" },
  { "en": "The two countries have a ___ relationship, marked by years of conflict.", "id": "volatile" },
  { "en": "The new policy is designed to ___ the transition to a green economy.", "id": "facilitate" },
  { "en": "The ancient text is full of ___ references that are difficult for modern readers to understand.", "id": "archaic" },
  { "en": "He is known for his ___ lifestyle, avoiding all modern technology.", "id": "ascetic" },
  { "en": "The CEO's ___ speech inspired the employees to work harder.", "id": "eloquent" },
  { "en": "The company is looking for ways to ___ its market share.", "id": "consolidate" },
  { "en": "The old laws are no longer ___ in today's society.", "id": "relevant" },
  { "en": "The politician made a ___ attempt to hide his involvement in the scandal.", "id": "blatant" },
  { "en": "The company's profits have been ___ for the past three years.", "id": "stagnant" },
  { "en": "The new evidence completely ___ the prosecution's case.", "id": "undermines" },
  { "en": "The two companies are ___ for the same government contract.", "id": "vying" },
  { "en": "The team is composed ___ of experts from various fields.", "id": "exclusively" },
  { "en": "The new law has been met with ___ opposition.", "id": "widespread" },
  { "en": "The government has a ___ interest in maintaining economic stability.", "id": "vested" },
  { "en": "The project was ___ from the beginning due to a lack of funding.", "id": "doomed" },
  { "en": "He has a ___ for dramatic storytelling.", "id": "flair" },
  { "en": "The two accounts of the event are ___ different.", "id": "fundamentally" },
  { "en": "The company's ___ into the European market was a huge success.", "id": "foray" },
  { "en": "The city is a ___ of activity during the festival.", "id": "hub" },
  { "en": "The witness's statement was ___ and provided no useful information.", "id": "nebulous" },
  { "en": "The country is on the ___ of a major political change.", "id": "brink" },
  { "en": "His ___ argument convinced the jury of his innocence.", "id": "plausible" },
  { "en": "The new policy is a ___ for disaster.", "id": "recipe" },
  { "en": "The company has a ___ on the local market.", "id": "monopoly" },
  { "en": "The ___ of the matter is that we must act now.", "id": "crux" },
  { "en": "The old traditions have been ___ by modern technology.", "id": "supplanted" },
  { "en": "The company is facing ___ financial difficulties.", "id": "dire" },
  { "en": "The author's latest novel has received ___ reviews from critics.", "id": "rave" },
  { "en": "The two leaders have a ___ respect for each other.", "id": "mutual" },
  { "en": "The project is a ___ of the company's commitment to innovation.", "id": "reflection" },
  { "en": "The government is trying to ___ foreign investment.", "id": "attract" },
  { "en": "The new CEO plans to ___ the company's structure.", "id": "overhaul" },
  { "en": "His work is ___ to the success of the entire team.", "id": "integral" },
  { "en": "The city's ___ includes a modern subway system and efficient buses.", "id": "infrastructure" },
  { "en": "The two issues are ___ and must be considered together.", "id": "intertwined" },
  { "en": "The witness's testimony was ___ from the official record.", "id": "stricken" },
  { "en": "The ___ of the problem is a lack of communication.", "id": "root" },
  { "en": "The new policy is a ___ departure from previous approaches.", "id": "radical" },
  { "en": "He is ___ to criticism and does not take feedback well.", "id": "sensitive" },
  { "en": "The company's profits ___ last quarter.", "id": "plummeted" },
  { "en": "The two candidates have ___ different visions for the country's future.", "id": "starkly" },
  { "en": "The city is a ___ of cultural diversity.", "id": "mosaic" },
  { "en": "The company is trying to ___ its losses after a difficult year.", "id": "recoup" },
  { "en": "His ___ to the details of the plan was impressive.", "id": "adherence" },
  { "en": "The new evidence ___ the defendant's alibi.", "id": "bolsters" },
  { "en": "The old castle is ___ in mystery.", "id": "shrouded" },
  { "en": "The decision was made to ___ the potential risks.", "id": "minimize" },
  { "en": "The two countries have agreed to ___ trade barriers.", "id": "dismantle" },
  { "en": "The company's ___ in the market has grown steadily.", "id": "foothold" },
  { "en": "His speech was a ___ call to action.", "id": "clarion" },
  { "en": "The evidence is ___ and does not prove anything.", "id": "circumstantial" },
  { "en": "The government plans to ___ the tax system.", "id": "reform" },
  { "en": "The project is a ___ to the team's creativity and hard work.", "id": "monument" },
  { "en": "The new policy is ___ to be controversial.", "id": "bound" },
  { "en": "He has a ___ for making friends easily.", "id": "penchant" },
  { "en": "The company's financial records are under ___ by auditors.", "id": "scrutiny" },
  { "en": "The two theories ___ on several key points.", "id": "converge" },
  { "en": "The ___ of the problem is that both sides refuse to compromise.", "id": "paradox" },
  { "en": "The company is on a ___ financial footing.", "id": "solid" },
  { "en": "His explanation was ___ and did not satisfy the committee.", "id": "inadequate" },
  { "en": "The new law will ___ a new era of economic prosperity.", "id": "usher in" },
  { "en": "Of the three brothers, he is the ___ at playing chess.", "id": "best" },
  { "en": "The situation is far ___ than we had anticipated.", "id": "worse" },
  { "en": "The new evidence took the investigation ___ than the previous leads.", "id": "further" },
  { "en": "This is by far the ___ interesting book I have read all year.", "id": "most" },
  { "en": "The problem was more complicated ___ we first imagined.", "id": "than" },
  { "en": "You can sit ___ you like; there are plenty of empty seats.", "id": "wherever" },
  { "en": "___ he says, you should not believe him because he often lies.", "id": "Whatever" },
  { "en": "Feel free to call me ___ you need help.", "id": "whenever" },
  { "en": "The company will hire ___ is best qualified for the job, regardless of their background.", "id": "whoever" },
  { "en": "He handles difficult situations with calm and professionalism, ___ happens.", "id": "whatever" },
  { "en": "We should deal with the problem at ___ before it gets worse.", "id": "hand" },
  { "en": "In the ___ run, investing in education will benefit the entire country.", "id": "long" },
  { "en": "Many people prefer to travel by ___ because it is more convenient.", "id": "car" },
  { "en": "As a ___ of fact, I have already finished the report.", "id": "matter" },
  { "en": "He decided to take matters into his own ___.", "id": "hands" },
  { "en": "He spoke slowly and clearly ___ everyone could understand him.", "id": "so that" },
  { "en": "The storm was ___ powerful that it knocked down trees.", "id": "so" },
  { "en": "She saved her money ___ she could buy a new laptop.", "id": "so that" },
  { "en": "There was ___ much traffic that we were an hour late.", "id": "so" },
  { "en": "He wore a disguise ___ no one would recognize him.", "id": "so that" },
  { "en": "After the long hike, the soup tasted ___.", "id": "wonderful" },
  { "en": "He feels ___ about his performance in the interview.", "id": "bad" },
  { "en": "The roses in the garden smell ___.", "id": "sweet" },
  { "en": "She looked at the data ___.", "id": "carefully" },
  { "en": "The music sounds too ___.", "id": "loud" },
  { "en": "The ___ results of the experiment were surprising and differed from expectations.", "id": "anomalous" },
  { "en": "The government provided aid to ___ the suffering of the flood victims.", "id": "assuage" },
  { "en": "The long, boring lecture seemed to ___ the students rather than inspire them.", "id": "enervate" },
  { "en": "Her efforts to help the homeless are truly ___.", "id": "laudable" },
  { "en": "The politician's speech was deliberately ___, leaving his true intentions unclear.", "id": "opaque" },
  { "en": "His ___ spending habits led him into serious debt.", "id": "prodigal" },
  { "en": "He is a ___ of the new policy, defending it against all criticism.", "id": "zealot" },
  { "en": "The doctor advised him to ___ from alcohol for several weeks.", "id": "abstain" },
  { "en": "It is illegal to ___ food products with cheaper, low-quality ingredients.", "id": "adulterate" },
  { "en": "She was ___ towards the election, feeling that her vote wouldn't make a difference.", "id": "apathetic" },
  { "en": "A key ___ of the plan is its reliance on unproven technology.", "id": "weakness" },
  { "en": "Neither the players nor the coach ___ satisfied with the referee's decision.", "id": "was" },
  { "en": "On the board ___ a list of instructions for the new employees.", "id": "was" },
  { "en": "It's high time you ___ responsibility for your actions.", "id": "took" },
  { "en": "The more he thought about it, ___ he became.", "id": "the more confused" },
  { "en": "I would have called you, but my phone ___.", "id": "was broken" },
  { "en": "She is one of the few people ___ I can truly trust.", "id": "whom" },
  { "en": "It was ___ an interesting lecture that everyone stayed until the very end.", "id": "such" },
  { "en": "Having ___ all the required courses, he is now eligible to graduate.", "id": "completed" },
  { "en": "The success of the project depends largely ___ the availability of funding.", "id": "on" },
  { "en": "Not only ___ a talented singer, but she is also a gifted actress.", "id": "is she" },
  { "en": "A number of students ___ having difficulty with the new online system.", "id": "are" },
  { "en": "He works too hard; ___, his health is starting to suffer.", "id": "consequently" },
  { "en": "He talks as if he ___ everything, but he actually knows very little.", "id": "knew" },
  { "en": "The report needs ___ before it is submitted.", "id": "to be reviewed" },
  { "en": "The company has an ___ reputation for quality and reliability.", "id": "impeccable" },
  { "en": "The old traditions have been ___ by modern customs.", "id": "eroded" },
  { "en": "The politician tried to ___ the issue by changing the subject.", "id": "deflect" },
  { "en": "The two companies are engaged in a ___ legal battle.", "id": "protracted" },
  { "en": "His ___ remarks made everyone in the room feel uncomfortable.", "id": "inappropriate" },
  { "en": "The documentary provides a ___ look at the lives of factory workers.", "id": "candid" },
  { "en": "The new evidence ___ the defendant's alibi.", "id": "corroborates" },
  { "en": "The company's failure was ___ due to poor management.", "id": "largely" },
  { "en": "The two cultures, ___ different, share some common values.", "id": "though" },
  { "en": "The project is a ___ to the team's perseverance.", "id": "testament" },
  { "en": "The new policy is designed to ___ innovation.", "id": "foster" },
  { "en": "He has a ___ for getting his own way.", "id": "knack" },
  { "en": "The ___ of the matter is that we are out of options.", "id": "reality" },
  { "en": "The new system is ___ with all previous versions of the software.", "id": "compatible" },
  { "en": "The country's economy has been ___ for several years.", "id": "sluggish" },
  { "en": "His ___ to the company's success is undeniable.", "id": "contribution" },
  { "en": "The two sides have reached a ___ understanding.", "id": "mutual" },
  { "en": "The new regulations will have a ___ impact on small businesses.", "id": "significant" },
  { "en": "He is widely ___ as the father of modern computing.", "id": "regarded" },
  { "en": "The company is taking ___ to reduce its carbon footprint.", "id": "measures" },
  { "en": "The witness gave a ___ account of the accident.", "id": "coherent" },
  { "en": "The old building is scheduled for ___.", "id": "demolition" },
  { "en": "The new discovery has opened up a whole new ___ of research.", "id": "avenue" },
  { "en": "The team's victory was a ___ of their hard work.", "id": "result" },
  { "en": "The government has a ___ to protect its citizens.", "id": "duty" },
  { "en": "His ___ is based on a misunderstanding of the facts.", "id": "argument" },
  { "en": "The project was completed ___ budget and ahead of schedule.", "id": "under" },
  { "en": "The two candidates have ___ different approaches to the problem.", "id": "completely" },
  { "en": "The company has a ___ of being a great place to work.", "id": "reputation" },
  { "en": "The new policy is a ___ in the right direction.", "id": "step" },
  { "en": "The evidence is ___ and does not point to a clear conclusion.", "id": "ambiguous" },
  { "en": "The company is ___ for its customer service.", "id": "renowned" },
  { "en": "The two countries have a ___ relationship.", "id": "strained" },
  { "en": "The new evidence ___ the original theory.", "id": "supports" },
  { "en": "The company's ___ is to provide high-quality products at affordable prices.", "id": "mission" },
  { "en": "The two designs are ___ the same.", "id": "essentially" },
  { "en": "The project is a ___ of several different ideas.", "id": "synthesis" },
  { "en": "The new regulations are ___ and difficult to understand.", "id": "complex" },
  { "en": "The company is taking a ___ approach to marketing.", "id": "new" },
  { "en": "The two sides are ___ to reach an agreement.", "id": "unable" },
  { "en": "The ___ of the problem is its complexity.", "id": "essence" },
  { "en": "The new system is ___ to be more secure.", "id": "designed" },
  { "en": "The two countries have a ___ of non-aggression.", "id": "pact" },
  { "en": "His work is a ___ contribution to the field.", "id": "significant" },
  { "en": "The company is ___ in a small town.", "id": "based" },
  { "en": "The two systems are ___ and cannot work together.", "id": "incompatible" },
  { "en": "The project is a ___ between public and private sectors.", "id": "partnership" },
  { "en": "The new policy is a ___ issue among the employees.", "id": "divisive" },
  { "en": "The company is taking a ___ risk by entering the new market.", "id": "calculated" },
  { "en": "The two leaders have a ___ to discuss the current crisis.", "id": "meeting" },
  { "en": "If the company ___ more in research and development, it would be a market leader now.", "id": "had invested" },
  { "en": "If you ___ the blue button, the machine turns on.", "id": "press" },
  { "en": "Should you ___ any problems, please do not hesitate to contact customer service.", "id": "encounter" },
  { "en": "I wouldn't be in this difficult situation if I ___ your advice.", "id": "had taken" },
  { "en": "If I ___ you, I would apologize for the mistake.", "id": "were" },
  { "en": "The report should ___ by the end of the week, but it's still not finished.", "id": "have been completed" },
  { "en": "This issue must ___ with extreme care.", "id": "be handled" },
  { "en": "More research could ___ in this area to verify the results.", "id": "be done" },
  { "en": "The package might ___ to the wrong address by mistake.", "id": "have been sent" },
  { "en": "The rules can't ___ without approval from the board.", "id": "be changed" },
  { "en": "___ the heavy rain, the game continued as planned.", "id": "Despite" },
  { "en": "He decided to go for a walk, ___ he was feeling tired.", "id": "even though" },
  { "en": "In spite ___ the difficulties, the team managed to succeed.", "id": "of" },
  { "en": "The new plan has some advantages, ___ it also has several major flaws.", "id": "although" },
  { "en": "She passed the exam with a high score ___ not studying very much.", "id": "despite" },
  { "en": "I decided to write an email ___ making a phone call.", "id": "instead of" },
  { "en": "He chose to resign ___ be fired from his position.", "id": "rather than" },
  { "en": "___ complaining about the problem, you should try to find a solution.", "id": "Instead of" },
  { "en": "She prefers walking ___ driving to work.", "id": "rather than" },
  { "en": "The new manager's policies will ___ the entire department.", "id": "affect" },
  { "en": "The positive ___ of the new medicine were immediately obvious.", "id": "effects" },
  { "en": "You should ___ the book on the table before you leave.", "id": "lay" },
  { "en": "The sun ___ in the east.", "id": "rises" },
  { "en": "Her blue scarf ___ her green eyes.", "id": "complements" },
  { "en": "Her decisions often seem ___, changing from one day to the next without reason.", "id": "capricious" },
  { "en": "The author's vivid descriptions ___ a feeling of nostalgia in the reader.", "id": "engender" },
  { "en": "He was too ___ and believed every promise the salesperson made.", "id": "gullible" },
  { "en": "The manager tried to ___ the angry employees by offering them a bonus.", "id": "placate" },
  { "en": "He tends to ___ between wanting to start a business and wanting a stable job.", "id": "vacillate" },
  { "en": "In many cultures, it is a tradition to ___ one's ancestors.", "id": "venerate" },
  { "en": "The professor's lecture was so ___ that half the class fell asleep.", "id": "soporific" },
  { "en": "The strange experimental result was considered an ___ by the research team.", "id": "aberration" },
  { "en": "She accepted the new assignment with ___, eager to begin.", "id": "alacrity" },
  { "en": "There is a ___ of evidence to support his theory, making it unconvincing.", "id": "paucity" },
  { "en": "___ the team's best player was injured, they still managed to win the game.", "id": "Although" },
  { "en": "A number of reasons ___ given for the project's delay.", "id": "were" },
  { "en": "Never ___ I heard such a ridiculous excuse.", "id": "have" },
  { "en": "It was ___ a beautiful day that we decided to have a picnic.", "id": "such" },
  { "en": "The committee, which ___ of ten members, meets once a month.", "id": "consists" },
  { "en": "I would rather you ___ me the truth.", "id": "told" },
  { "en": "He is responsible for ___ the entire department.", "id": "managing" },
  { "en": "This problem needs ___ immediately.", "id": "to be solved" },
  { "en": "The more he studies, ___ he realizes he needs to learn.", "id": "the more" },
  { "en": "By the time the fire brigade arrived, the building ___ already burned down.", "id": "had" },
  { "en": "It is imperative that every employee ___ the safety regulations.", "id": "follow" },
  { "en": "I am not sure about ___ to invite to the party.", "id": "whom" },
  { "en": "This is the university ___ my parents first met.", "id": "where" },
  { "en": "___ of the bad weather, all flights have been canceled.", "id": "Because" },
  { "en": "He is not only an excellent scientist ___ a gifted musician.", "id": "but also" },
  { "en": "Her expertise lies ___ the field of corporate law.", "id": "within" },
  { "en": "The company's main ___ is to increase market share.", "id": "objective" },
  { "en": "The new evidence casts ___ on the defendant's story.", "id": "doubt" },
  { "en": "The two countries have a ___ relationship.", "id": "stable" },
  { "en": "The project was a ___ effort between several organizations.", "id": "joint" },
  { "en": "The ___ of the matter is that we are behind schedule.", "id": "crux" },
  { "en": "The new regulations are intended to ___ fair competition.", "id": "ensure" },
  { "en": "His work is ___ and always of the highest quality.", "id": "meticulous" },
  { "en": "The company's profits ___ by 20% last quarter.", "id": "increased" },
  { "en": "She has a ___ for remembering historical dates.", "id": "talent" },
  { "en": "The new system is far superior ___ the old one.", "id": "to" },
  { "en": "The government is facing pressure to ___ the problem.", "id": "address" },
  { "en": "The witness gave a ___ account of what happened.", "id": "factual" },
  { "en": "The new policy is a ___ shift from the old one.", "id": "significant" },
  { "en": "The two departments must ___ to achieve the company's goals.", "id": "cooperate" },
  { "en": "The ___ of the study were published in a medical journal.", "id": "findings" },
  { "en": "He is a ___ supporter of the environmental cause.", "id": "staunch" },
  { "en": "The new evidence makes his story seem more ___.", "id": "plausible" },
  { "en": "The company has a ___ of being an innovative leader.", "id": "reputation" },
  { "en": "The ___ of the project will be determined by its profitability.", "id": "success" },
  { "en": "The new drug has shown ___ results in early trials.", "id": "promising" },
  { "en": "The two countries have a ___ agreement.", "id": "bilateral" },
  { "en": "The company is required to ___ with federal regulations.", "id": "comply" },
  { "en": "His ___ to the team is invaluable.", "id": "dedication" },
  { "en": "The new system is ___ to be more user-friendly.", "id": "intended" },
  { "en": "The two systems are ___ in function but different in design.", "id": "similar" },
  { "en": "The project was completed ___ schedule.", "id": "on" },
  { "en": "The new evidence ___ the defendant from all blame.", "id": "absolves" },
  { "en": "The company's ___ are to increase profit and expand its market.", "id": "objectives" },
  { "en": "The two sides are ___ to reach a compromise.", "id": "trying" },
  { "en": "The new policy has ___ a lot of debate.", "id": "generated" },
  { "en": "The company's performance has been ___ expectations.", "id": "below" },
  { "en": "The two leaders have a ___ of views on the issue.", "id": "difference" },
  { "en": "The new system is a ___ improvement.", "id": "major" },
  { "en": "The company is known for its ___ business practices.", "id": "ethical" },
  { "en": "The two departments work in ___ with each other.", "id": "conjunction" },
  { "en": "The project is a ___ between the government and private industry.", "id": "collaboration" },
  { "en": "The new regulations are ___ and confusing.", "id": "ambiguous" },
  { "en": "The company is taking a ___ approach to risk management.", "id": "cautious" },
  { "en": "The two countries have a ___ relationship based on trade.", "id": "strong" },
  { "en": "The ___ of the problem is a lack of funding.", "id": "core" },
  { "en": "The new system is designed to be ___ and easy to use.", "id": "intuitive" },
  { "en": "The two companies have ___ their resources to work on the project.", "id": "pooled" },
  { "en": "His work is a ___ example of modern art.", "id": "prime" },
  { "en": "The company is ___ in London.", "id": "headquartered" },
  { "en": "The two systems are ___ dependent on each other.", "id": "mutually" },
  { "en": "The project is a ___ for future development.", "id": "blueprint" },
  { "en": "The new policy is a ___ issue.", "id": "contentious" },
  { "en": "The company is taking a ___ step to enter the new market.", "id": "bold" },
  { "en": "The two leaders have a ___ to meet annually.", "id": "commitment" },
  { "en": "The plot of the movie was very ___, and it kept me on the edge of my seat.", "id": "exciting" },
  { "en": "I was ___ to hear that I had won the competition.", "id": "thrilled" },
  { "en": "The professor's lecture on quantum physics was ___.", "id": "fascinating" },
  { "en": "After working a 12-hour shift, the nurses were completely ___.", "id": "exhausted" },
  { "en": "The instructions for the new software are quite ___.", "id": "confusing" },
  { "en": "You will not be allowed to take the exam ___ you have a valid ID.", "id": "unless" },
  { "en": "___ he is known for being quiet, he is actually quite talkative among his friends.", "id": "While" },
  { "en": "___ the company is based in the US, it has offices all over the world.", "id": "Although" },
  { "en": "___ you have already completed the prerequisite courses, you may register for the advanced class.", "id": "Since" },
  { "en": "He is a respected expert, ___ his colleague is still a novice.", "id": "whereas" },
  { "en": "The museum features works by famous impressionist painters, ___ Monet and Renoir.", "id": "such as" },
  { "en": "He enjoys outdoor activities ___ hiking and camping.", "id": "like" },
  { "en": "Many countries, ___ Japan and Germany, have aging populations.", "id": "such as" },
  { "en": "A variety of topics ___ economics and political science will be discussed.", "id": "such as" },
  { "en": "The committee ___ scheduled its next meeting for Tuesday.", "id": "has" },
  { "en": "The jury ___ still deliberating, unable to agree on a verdict.", "id": "are" },
  { "en": "The faculty ___ unanimously approved the new proposal.", "id": "has" },
  { "en": "The team ___ celebrating their victory in the locker room.", "id": "is" },
  { "en": "The public ___ divided in their opinion of the new law.", "id": "is" },
  { "en": "He worked ___ to meet the deadline.", "id": "diligently in his office all week" },
  { "en": "The birds migrate ___ every year.", "id": "south in the winter" },
  { "en": "She spoke ___ during the presentation.", "id": "confidently at the conference yesterday" },
  { "en": "The package arrived ___ .", "id": "safely at my doorstep this morning" },
  { "en": "They rehearsed ___ for the play.", "id": "tirelessly in the studio for weeks" },
  { "en": "The teacher made the students ___ their essays.", "id": "rewrite" },
  { "en": "I need to have my passport ___.", "id": "renewed" },
  { "en": "She let her son ___ to the party.", "id": "go" },
  { "en": "He got a professional ___ his house.", "id": "to paint" },
  { "en": "The director had the report ___ by his assistant.", "id": "typed" },
  { "en": "His argument that the Earth is flat is a clear ___ in the 21st century.", "id": "anachronism" },
  { "en": "The crowd was ___ after their team won the championship.", "id": "boisterous" },
  { "en": "There is a strong sense of ___ among the members of the team.", "id": "camaraderie" },
  { "en": "The professor's lecture was interesting, but he made a long ___ about his vacation.", "id": "digression" },
  { "en": "The beauty of the cherry blossoms is ___, lasting only for a week or two.", "id": "evanescent" },
  { "en": "Known for his ___, he lived a simple life and saved most of his money.", "id": "frugality" },
  { "en": "He is a ___ person who enjoys parties and social gatherings.", "id": "gregarious" },
  { "en": "The coach delivered a fiery ___ to the team during halftime.", "id": "harangue" },
  { "en": "His ___ decision to quit his job without another one lined up worried his family.", "id": "impetuous" },
  { "en": "The soup was ___, lacking any real flavor.", "id": "insipid" },
  { "en": "The number of people attending the conference ___ smaller than expected.", "id": "was" },
  { "en": "If he ___ about the meeting, he would have been there.", "id": "had known" },
  { "en": "The problem is far more serious than ___.", "id": "it appears" },
  { "en": "___ the bad weather, we enjoyed our vacation.", "id": "In spite of" },
  { "en": "He is considering ___ his job and starting his own business.", "id": "leaving" },
  { "en": "The manager, to ___ all employees report, will be retiring soon.", "id": "whom" },
  { "en": "So loudly ___ that the neighbors complained.", "id": "did he shout" },
  { "en": "It's time the children ___ in bed.", "id": "were" },
  { "en": "She is not used to ___ in such a large city.", "id": "living" },
  { "en": "Hardly had the sun set ___ it began to rain heavily.", "id": "when" },
  { "en": "His speech was designed to ___ support for his campaign.", "id": "garner" },
  { "en": "The professor's ___ style made complex subjects easy to understand.", "id": "lucid" },
  { "en": "The ___ of the matter is that we have a deadline to meet.", "id": "reality" },
  { "en": "The new evidence ___ the original theory.", "id": "refutes" },
  { "en": "The company's ___ to safety is its top priority.", "id": "commitment" },
  { "en": "The project was a ___ success, praised by critics and the public alike.", "id": "resounding" },
  { "en": "The politician's comments were a ___ attempt to win votes.", "id": "transparent" },
  { "en": "The two countries have a ___ relationship, often cooperating on key issues.", "id": "cordial" },
  { "en": "The manager is ___ for all final decisions.", "id": "accountable" },
  { "en": "The artist's work is a ___ of his personal struggles.", "id": "manifestation" },
  { "en": "The new law is intended to ___ the rights of minorities.", "id": "safeguard" },
  { "en": "His argument was based on ___ logic.", "id": "flawed" },
  { "en": "The ___ of the company is located in New York.", "id": "headquarters" },
  { "en": "The two systems are ___ and serve different purposes.", "id": "distinct" },
  { "en": "The company has ___ a new approach to customer service.", "id": "adopted" },
  { "en": "His ___ to the rules is very strict.", "id": "adherence" },
  { "en": "The two leaders have a ___ to meet regularly.", "id": "commitment" },
  { "en": "The new evidence ___ his innocence.", "id": "proves" },
  { "en": "The company is a ___ in the field of renewable energy.", "id": "pioneer" },
  { "en": "The two designs are ___ in many respects.", "id": "analogous" },
  { "en": "The project is a ___ of international cooperation.", "id": "model" },
  { "en": "The new regulations are ___ for all employees.", "id": "mandatory" },
  { "en": "His ___ to detail is a great asset.", "id": "attention" },
  { "en": "The new system is designed to ___ operations.", "id": "streamline" },
  { "en": "The two countries have a ___ history of conflict.", "id": "complex" },
  { "en": "The project is a ___ success.", "id": "monumental" },
  { "en": "The new evidence is ___ with the defendant's story.", "id": "consistent" },
  { "en": "The company is a ___ of the local community.", "id": "pillar" },
  { "en": "The two designs are ___ different.", "id": "stylistically" },
  { "en": "The project is a ___ of many people's hard work.", "id": "product" },
  { "en": "The new regulations are ___ and will take effect next month.", "id": "final" },
  { "en": "The two companies are ___ on a new project.", "id": "collaborating" },
  { "en": "His ___ to the team has been crucial.", "id": "value" },
  { "en": "The new system is ___ to be more reliable.", "id": "supposed" },
  { "en": "The two countries have a ___ relationship.", "id": "cooperative" },
  { "en": "The project is a ___ of modern engineering.", "id": "marvel" },
  { "en": "The new regulations are ___ to follow.", "id": "easy" },
  { "en": "The two companies have a ___ to compete fairly.", "id": "pact" },
  { "en": "His work is ___ of his dedication.", "id": "indicative" },
  { "en": "The project is a ___ success story.", "id": "classic" },
  { "en": "The new system is designed to be ___ with mobile devices.", "id": "compatible" },
  { "en": "The two countries have ___ diplomatic ties.", "id": "severed" },
  { "en": "His ___ of the situation was very accurate.", "id": "assessment" },
  { "en": "The new policy is a ___ of the company's new direction.", "id": "sign" },
  { "en": "The two systems are ___ linked.", "id": "inextricably" },
  { "en": "The project is a ___ of our commitment to sustainability.", "id": "demonstration" },
  { "en": "The new regulations are ___ and must be followed.", "id": "binding" },
  { "en": "The two companies have a ___ for innovation.", "id": "reputation" },
  { "en": "His work is a ___ of modern art.", "id": "masterpiece" },
  { "en": "___ he practices, the more confident he becomes.", "id": "The more" },
  { "en": "The sooner we leave, ___ we will arrive.", "id": "the earlier" },
  { "en": "The more complex the problem, ___ it is to solve.", "id": "the harder" },
  { "en": "The less you worry about it, ___ you will feel.", "id": "the better" },
  { "en": "___ the population grows, the more resources are needed.", "id": "The larger" },
  { "en": "___ he was able to finish the project so quickly remains a mystery.", "id": "How" },
  { "en": "I am interested in ___ the new policy will affect our department.", "id": "how" },
  { "en": "We need to decide ___ we should proceed.", "id": "whether" },
  { "en": "___ caused the power outage is still under investigation.", "id": "What" },
  { "en": "Can you explain ___ you were late?", "id": "why" },
  { "en": "He works ___ a consultant for a major tech firm.", "id": "as" },
  { "en": "___ I was walking home, it began to snow.", "id": "As" },
  { "en": "___ you know, the deadline has been extended by one week.", "id": "As" },
  { "en": "He is not as tall ___ his brother.", "id": "as" },
  { "en": "___ the company was losing money, they had to lay off some employees.", "id": "As" },
  { "en": "The manager demanded that the report ___ on her desk by 5 PM.", "id": "be" },
  { "en": "I suggest that he ___ to a specialist for a second opinion.", "id": "go" },
  { "en": "The committee recommended that the proposal ___ rejected.", "id": "be" },
  { "en": "It is crucial that she ___ all the facts before making a decision.", "id": "have" },
  { "en": "The professor insisted that the students ___ their sources properly.", "id": "cite" },
  { "en": "By this time next year, the new bridge ___.", "id": "will have been built" },
  { "en": "The problem ___ for weeks, but no solution has been found yet.", "id": "has been discussed" },
  { "en": "While the new system ___, the old one will remain in operation.", "id": "is being installed" },
  { "en": "When I arrived, the documents ___ by the manager.", "id": "were being signed" },
  { "en": "If the problem had been identified earlier, it could ___.", "id": "have been solved" },
  { "en": "His ___ comments were intended to wound and criticize.", "id": "acerbic" },
  { "en": "The dictator sought to ___ his power by controlling the media.", "id": "aggrandize" },
  { "en": "The ___ organization donates millions to charity each year.", "id": "benevolent" },
  { "en": "The new regulations ___ the authority of local governments.", "id": "circumscribe" },
  { "en": "He launched into a long ___ against the company's policies.", "id": "diatribe" },
  { "en": "The ___ tour guide talked nonstop for the entire three-hour trip.", "id": "garrulous" },
  { "en": "The team's ___ defeat in the final match was a shock to their fans.", "id": "ignominious" },
  { "en": "He was so ___ that he agreed to every request and flattered his boss constantly.", "id": "obsequious" },
  { "en": "After the political turmoil, the country entered a ___ period of peace.", "id": "quiescent" },
  { "en": "He felt a deep-seated ___ towards his old rival.", "id": "rancor" },
  { "en": "If I ___ more time, I would learn another language.", "id": "had" },
  { "en": "The number of books in the library ___ grown significantly.", "id": "has" },
  { "en": "Scarcely had I finished my dinner ___ the phone began to ring.", "id": "when" },
  { "en": "This is the town in ___ I was born.", "id": "which" },
  { "en": "It was ___ cold that we decided to stay indoors.", "id": "so" },
  { "en": "You had better ___ a doctor about that cough.", "id": "see" },
  { "en": "In spite ___ feeling unwell, she went to work.", "id": "of" },
  { "en": "I'm not tired, and ___ is my friend.", "id": "neither" },
  { "en": "The lecture was so ___ that I could hardly stay awake.", "id": "boring" },
  { "en": "A great deal of research ___ been done on this topic.", "id": "has" },
  { "en": "That is the man ___ son won the scholarship.", "id": "whose" },
  { "en": "It's important for the project ___ on time.", "id": "to be finished" },
  { "en": "___ the economy has improved, many people are still struggling.", "id": "Although" },
  { "en": "The instructions were not clear ___.", "id": "enough" },
  { "en": "He stopped ___ a cup of coffee on his way to work.", "id": "to get" },
  { "en": "His work is ___ praised by critics and academics.", "id": "widely" },
  { "en": "The new policy is a ___ departure from the past.", "id": "radical" },
  { "en": "The two countries have a ___ of shared history.", "id": "legacy" },
  { "en": "The results of the study were ___ and could not be trusted.", "id": "invalid" },
  { "en": "The company has a ___ approach to employee training.", "id": "systematic" },
  { "en": "His ___ to the cause was never in doubt.", "id": "loyalty" },
  { "en": "The new evidence ___ the defendant's story.", "id": "contradicts" },
  { "en": "The two systems are ___ in their design.", "id": "identical" },
  { "en": "The project is a ___ of the team's creative talents.", "id": "showcase" },
  { "en": "The new regulations are ___ to prevent accidents.", "id": "designed" },
  { "en": "The two countries have a ___ relationship.", "id": "friendly" },
  { "en": "The ___ of the problem is its scale.", "id": "magnitude" },
  { "en": "The new evidence ___ him as a suspect.", "id": "implicates" },
  { "en": "The two systems are ___ but not the same.", "id": "similar" },
  { "en": "The project is a ___ to complete in such a short time.", "id": "challenge" },
  { "en": "The new regulations are ___ for all businesses.", "id": "compulsory" },
  { "en": "The two companies have a ___ to work together.", "id": "plan" },
  { "en": "His ___ of the topic is impressive.", "id": "knowledge" },
  { "en": "The new system is ___ to be implemented next year.", "id": "scheduled" },
  { "en": "The two countries have a ___ of cultural exchange.", "id": "tradition" },
  { "en": "The project is a ___ undertaking.", "id": "massive" },
  { "en": "The new regulations are ___ to understand.", "id": "difficult" },
  { "en": "The two companies have a ___ to share profits.", "id": "deal" },
  { "en": "His ___ is well-known in the industry.", "id": "expertise" },
  { "en": "The new system is ___ to be a major improvement.", "id": "expected" },
  { "en": "The two countries have a ___ relationship.", "id": "turbulent" },
  { "en": "The project is a ___ for innovation in the field.", "id": "catalyst" },
  { "en": "The new regulations are ___ and will be enforced strictly.", "id": "clear" },
  { "en": "The two companies have ___ an agreement.", "id": "reached" },
  { "en": "His ___ to the project was crucial for its success.", "id": "involvement" },
  { "en": "The new system is ___ to be more efficient.", "id": "guaranteed" },
  { "en": "The two countries have a ___ on most issues.", "id": "consensus" },
  { "en": "The project is a ___ of hard work and dedication.", "id": "product" },
  { "en": "The new regulations are ___ to many small businesses.", "id": "harmful" },
  { "en": "The two companies have a ___ to merge.", "id": "proposal" },
  { "en": "His ___ is evident in his work.", "id": "talent" },
  { "en": "The new system is ___ to be revolutionary.", "id": "considered" },
  { "en": "The two countries have a ___ of mutual defense.", "id": "treaty" },
  { "en": "The project is a ___ of teamwork.", "id": "triumph" },
  { "en": "The new regulations are ___ to interpret.", "id": "hard" },
  { "en": "The two companies have a ___ of interest.", "id": "conflict" },
  { "en": "His ___ in the matter is clear.", "id": "position" },
  { "en": "The new system is ___ to be in place by January.", "id": "due" },
  { "en": "The two countries have a ___ to resolve the dispute peacefully.", "id": "desire" },
  { "en": "The project is a ___ of our company's values.", "id": "reflection" },
  { "en": "The new regulations are ___ and need to be revised.", "id": "flawed" },
  { "en": "The two companies have a ___ to dominate the market.", "id": "strategy" },
  { "en": "His ___ is highly respected.", "id": "opinion" },
  { "en": "The new system is ___ to be operational next month.", "id": "slated" },
  { "en": "The two countries have a ___ to improve relations.", "id": "commitment" },
  { "en": "Seldom ___ a more beautiful sunset.", "id": "have I seen" },
  { "en": "Only after the storm had passed ___ the true extent of the damage.", "id": "did we see" },
  { "en": "Such ___ the power of his speech that the entire audience was moved to tears.", "id": "was" },
  { "en": "Not a single word ___ she say during the entire meeting.", "id": "did" },
  { "en": "Had it not been for the quick response of the firefighters, the building ___ have been destroyed.", "id": "would" },
  { "en": "The ideal candidate should be experienced, reliable, and ___ to work in a team.", "id": "able" },
  { "en": "The course teaches not only how to write code but also ___ it effectively.", "id": "how to debug" },
  { "en": "She enjoys both reading novels and ___ short stories.", "id": "writing" },
  { "en": "It is often easier to ask for forgiveness ___ for permission.", "id": "than to ask" },
  { "en": "The collection of rare stamps ___ worth a considerable amount of money.", "id": "is" },
  { "en": "Neither of the proposed solutions ___ to be effective.", "id": "seems" },
  { "en": "The news from the front lines ___ not encouraging.", "id": "is" },
  { "en": "Each of the team members ___ given a specific task.", "id": "was" },
  { "en": "The phenomena observed during the experiment ___ still not fully understood.", "id": "are" },
  { "en": "He is committed ___ the highest standards of quality.", "id": "to maintaining" },
  { "en": "I regret ___ you that your application has been unsuccessful.", "id": "to inform" },
  { "en": "It is no use ___ over spilled milk.", "id": "crying" },
  { "en": "The company has decided ___ its headquarters to a new city.", "id": "to move" },
  { "en": "She is capable ___ complex tasks under pressure.", "id": "of handling" },
  { "en": "Every employee is responsible for keeping ___ own workspace clean.", "id": "their" },
  { "en": "The board of directors will announce ___ decision tomorrow.", "id": "its" },
  { "en": "The books on the shelf, ___ are covered in dust, have not been touched for years.", "id": "which" },
  { "en": "Can you please give this report to ___?", "id": "him" },
  { "en": "It was ___ who designed the award-winning building.", "id": "they" },
  { "en": "As the storm began to ___, we were able to see the horizon again.", "id": "abate" },
  { "en": "He dreamed of a quiet, ___ life in the countryside, far from the noise of the city.", "id": "bucolic" },
  { "en": "He used legal ___ to avoid paying his taxes for years.", "id": "chicanery" },
  { "en": "Despite his expertise, he spoke with great ___ about his accomplishments.", "id": "diffidence" },
  { "en": "The crowd was ___ after the home team scored the winning goal.", "id": "ebullient" },
  { "en": "She was a ___ supporter of the cause, donating both time and money.", "id": "fervid" },
  { "en": "It is impossible to ___ the fact that the company has been losing money.", "id": "gainsay" },
  { "en": "The company has achieved cultural ___ with its products being used worldwide.", "id": "hegemony" },
  { "en": "His plans were still ___ and not yet ready to be presented.", "id": "inchoate" },
  { "en": "His ___ reply of 'maybe' was not helpful.", "id": "laconic" },
  { "en": "___ of the budget cuts, several programs will be eliminated.", "id": "As a result" },
  { "en": "The more options you have, ___ it can be to make a decision.", "id": "the harder" },
  { "en": "All luggage must ___ before boarding the plane.", "id": "be checked" },
  { "en": "Had I seen the warning sign, I ___ that road.", "id": "would not have taken" },
  { "en": "The committee, as well as the manager, ___ the proposal.", "id": "supports" },
  { "en": "He not only finished the race but also ___ a new record.", "id": "set" },
  { "en": "It is recommended that she ___ a break from her work.", "id": "take" },
  { "en": "He is interested ___ how the new technology works.", "id": "in" },
  { "en": "___ the project was difficult, it was also very rewarding.", "id": "While" },
  { "en": "The paintings, ___ were created in the 17th century, are priceless.", "id": "which" },
  { "en": "A number of the participants ___ from out of the country.", "id": "come" },
  { "en": "It was so late ___ we decided to take a taxi home.", "id": "that" },
  { "en": "I wish I ___ how to play the piano.", "id": "knew" },
  { "en": "The quality of the students' work ___ improved this semester.", "id": "has" },
  { "en": "She went to the store ___ some milk.", "id": "to buy" },
  { "en": "The ___ of the matter is that we cannot afford it.", "id": "truth" },
  { "en": "The new policy is a ___ for the company.", "id": "milestone" },
  { "en": "His work is ___ and often cited by other researchers.", "id": "influential" },
  { "en": "The two countries have a ___ relationship.", "id": "peaceful" },
  { "en": "The project is a ___ of the team's dedication.", "id": "symbol" },
  { "en": "The new regulations are ___ to be implemented next year.", "id": "set" },
  { "en": "The two companies have a ___ to develop new technologies.", "id": "partnership" },
  { "en": "His ___ to the problem was very innovative.", "id": "approach" },
  { "en": "The new system is ___ to be a game-changer.", "id": "poised" },
  { "en": "The two countries have a ___ history of cooperation.", "id": "long" },
  { "en": "The project is a ___ to the power of collaboration.", "id": "testament" },
  { "en": "The new regulations are ___ and will be difficult to enforce.", "id": "controversial" },
  { "en": "His ___ is respected throughout the industry.", "id": "judgment" },
  { "en": "The new system is ___ to be more secure than the old one.", "id": "designed" },
  { "en": "The two countries have a ___ trade relationship.", "id": "robust" },
  { "en": "The project is a ___ of the company's vision.", "id": "cornerstone" },
  { "en": "The new regulations are ___ and need to be simplified.", "id": "cumbersome" },
  { "en": "His ___ of the events was very clear.", "id": "recollection" },
  { "en": "The new system is ___ to be a major step forward.", "id": "seen" },
  { "en": "The two countries have a ___ of mutual respect.", "id": "foundation" },
  { "en": "The project is a ___ of our commitment to excellence.", "id": "manifestation" },
  { "en": "The new regulations are ___ to change.", "id": "subject" },
  { "en": "The two companies have a ___ to share research.", "id": "contract" },
  { "en": "His ___ is beyond question.", "id": "integrity" },
  { "en": "The new system is ___ to be more efficient.", "id": "proven" },
  { "en": "The two countries have a ___ that needs to be resolved.", "id": "dispute" },
  { "en": "The project is a ___ of innovative design.", "id": "masterpiece" },
  { "en": "The new regulations are ___ and open to interpretation.", "id": "vague" },
  { "en": "The two companies have a ___ to not hire each other's employees.", "id": "tacit agreement" },
  { "en": "His ___ on the matter is well-documented.", "id": "stance" },
  { "en": "The new system is ___ to fail without proper funding.", "id": "likely" },
  { "en": "The two countries have a ___ to protect the environment.", "id": "joint commitment" },
  { "en": "The project is a ___ of our company's capabilities.", "id": "demonstration" },
  { "en": "The new regulations are ___ and will be implemented soon.", "id": "forthcoming" },
  { "en": "The two companies have a ___ to innovate.", "id": "mandate" },
  { "en": "His ___ is highly valued by the team.", "id": "input" },
  { "en": "The new system is ___ to be a significant investment.", "id": "certain" },
  { "en": "The two countries have a ___ on border control.", "id": "protocol" },
  { "en": "The project is a ___ of our long-term strategy.", "id": "key component" },
  { "en": "The new regulations are ___ to be challenged in court.", "id": "likely" },
  { "en": "The two companies have a ___ to explore a merger.", "id": "tentative plan" },
  { "en": "His ___ is a valuable asset to the company.", "id": "experience" },
  { "en": "The new system is ___ to be launched in the spring.", "id": "on track" },
  { "en": "The two countries have a ___ to promote cultural exchange.", "id": "common goal" },
  { "en": "The project is a ___ of our company's innovation.", "id": "shining example" },
  { "en": "The new regulations are ___ and need further clarification.", "id": "unclear" },
  { "en": "The two companies have a ___ to keep the negotiations secret.", "id": "vested interest" },
  { "en": "His ___ is evident in the quality of his work.", "id": "craftsmanship" }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
