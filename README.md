<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcard การศึกษา - Eng/Thai</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            max-width: 900px;
            width: 100%;
            padding: 40px;
        }

        h1 {
            text-align: center;
            color: #667eea;
            margin-bottom: 10px;
            font-size: 2.5em;
        }

        .subtitle {
            text-align: center;
            color: #666;
            margin-bottom: 30px;
        }

        .mode-selector {
            display: flex;
            gap: 20px;
            justify-content: center;
            margin-bottom: 30px;
        }

        .mode-btn {
            padding: 15px 30px;
            font-size: 1.1em;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
        }

        .mode-btn.teacher {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
        }

        .mode-btn.student {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
        }

        .mode-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }

        .mode-btn.active {
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            transform: scale(1.05);
        }

        .category-selector {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 30px;
        }

        .category-btn {
            padding: 15px;
            border: 2px solid #667eea;
            background: white;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s;
            font-size: 1em;
            color: #667eea;
            font-weight: 600;
        }

        .category-btn:hover {
            background: #667eea;
            color: white;
            transform: translateY(-2px);
        }

        .category-btn.active {
            background: #667eea;
            color: white;
        }

        .flashcard-container {
            perspective: 1000px;
            margin-bottom: 30px;
        }

        .flashcard {
            width: 100%;
            height: 400px;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.6s;
            cursor: pointer;
        }

        .flashcard.flipped {
            transform: rotateY(180deg);
        }

        .card-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            border-radius: 15px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 40px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }

        .card-front {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .card-back {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
            transform: rotateY(180deg);
        }

        .card-text {
            font-size: 2.5em;
            font-weight: bold;
            text-align: center;
            word-wrap: break-word;
        }

        .card-hint {
            margin-top: 20px;
            font-size: 1em;
            opacity: 0.9;
            text-align: center;
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .control-btn {
            padding: 12px 25px;
            border: none;
            border-radius: 8px;
            background: #667eea;
            color: white;
            font-size: 1em;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
        }

        .control-btn:hover {
            background: #764ba2;
            transform: translateY(-2px);
        }

        .progress {
            text-align: center;
            font-size: 1.2em;
            color: #667eea;
            font-weight: bold;
        }

        .hidden {
            display: none;
        }

        @media (max-width: 768px) {
            .container {
                padding: 20px;
            }

            h1 {
                font-size: 1.8em;
            }

            .card-text {
                font-size: 1.8em;
            }

            .mode-selector {
                flex-direction: column;
            }

            .category-selector {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📚 Flashcard การศึกษา</h1>
        <p class="subtitle">English - ไทย</p>

        <div class="mode-selector">
            <button class="mode-btn teacher" onclick="selectMode('teacher')">👨‍🏫 สำหรับครู/ผู้บริหาร</button>
            <button class="mode-btn student" onclick="selectMode('student')">🎓 สำหรับนักเรียน</button>
        </div>

        <div id="categorySection" class="hidden">
            <div id="categorySelector" class="category-selector"></div>
        </div>

        <div id="flashcardSection" class="hidden">
            <div class="flashcard-container">
                <div class="flashcard" id="flashcard" onclick="flipCard()">
                    <div class="card-face card-front">
                        <div class="card-text" id="frontText"></div>
                        <div class="card-hint">คลิกเพื่อดูคำตอบ</div>
                    </div>
                    <div class="card-face card-back">
                        <div class="card-text" id="backText"></div>
                        <div class="card-hint">คลิกเพื่อกลับ</div>
                    </div>
                </div>
            </div>

            <div class="controls">
                <button class="control-btn" onclick="previousCard()">◀ ก่อนหน้า</button>
                <button class="control-btn" onclick="toggleLanguage()">🔄 สลับภาษา</button>
                <button class="control-btn" onclick="shuffleCards()">🔀 สุ่มการ์ด</button>
                <button class="control-btn" onclick="nextCard()">ถัดไป ▶</button>
            </div>

            <div class="progress" id="progress"></div>
        </div>
    </div>

    <script>
        const flashcards = {
            teacher: {
                'การบริหารจัดการ': [
                    { en: 'Administration', th: 'การบริหาร', hint: 'การจัดการองค์กร' },
                    { en: 'Curriculum', th: 'หลักสูตร', hint: 'แผนการเรียน' },
                    { en: 'Assessment', th: 'การประเมิน', hint: 'การวัดผล' },
                    { en: 'Professional Development', th: 'การพัฒนาวิชาชีพ', hint: 'การเพิ่มทักษะครู' },
                    { en: 'School Policy', th: 'นโยบายโรงเรียน', hint: 'กฎระเบียบ' },
                    { en: 'Budget Management', th: 'การจัดการงบประมาณ', hint: 'การเงิน' },
                    { en: 'Strategic Planning', th: 'การวางแผนเชิงกลยุทธ์', hint: 'แผนระยะยาว' },
                    { en: 'Quality Assurance', th: 'การประกันคุณภาพ', hint: 'มาตรฐาน' }
                ],
                'การสอนและพัฒนา': [
                    { en: 'Pedagogy', th: 'วิธีการสอน', hint: 'ศาสตร์การสอน' },
                    { en: 'Learning Outcomes', th: 'ผลการเรียนรู้', hint: 'เป้าหมายการเรียน' },
                    { en: 'Differentiated Instruction', th: 'การสอนแบบหลากหลาย', hint: 'ตามความสามารถนักเรียน' },
                    { en: 'Classroom Management', th: 'การจัดการชั้นเรียน', hint: 'การควบคุมห้องเรียน' },
                    { en: 'Formative Assessment', th: 'การประเมินเพื่อพัฒนา', hint: 'ประเมินระหว่างเรียน' },
                    { en: 'Summative Assessment', th: 'การประเมินผลรวม', hint: 'ประเมินปลายภาค' },
                    { en: 'Rubric', th: 'เกณฑ์การประเมิน', hint: 'มาตรวัดคุณภาพ' },
                    { en: 'Lesson Plan', th: 'แผนการสอน', hint: 'แผนบทเรียน' }
                ],
                'ภาวะผู้นำ': [
                    { en: 'Leadership', th: 'ภาวะผู้นำ', hint: 'การนำ' },
                    { en: 'Vision and Mission', th: 'วิสัยทัศน์และพันธกิจ', hint: 'เป้าหมายองค์กร' },
                    { en: 'Stakeholder', th: 'ผู้มีส่วนได้ส่วนเสีย', hint: 'ผู้เกี่ยวข้อง' },
                    { en: 'Collaboration', th: 'ความร่วมมือ', hint: 'การทำงานร่วมกัน' },
                    { en: 'Innovation', th: 'นวัตกรรม', hint: 'สิ่งใหม่' },
                    { en: 'Accountability', th: 'ความรับผิดชอบ', hint: 'ตอบสนองผลงาน' },
                    { en: 'Empowerment', th: 'การเสริมสร้างพลัง', hint: 'มอบอำนาจ' },
                    { en: 'Mentoring', th: 'การให้คำปรึกษา', hint: 'การแนะนำ' }
                ],
                'การสื่อสาร': [
                    { en: 'Parent Engagement', th: 'การมีส่วนร่วมของผู้ปกครอง', hint: 'ความเกี่ยวข้องของพ่อแม่' },
                    { en: 'Communication', th: 'การสื่อสาร', hint: 'การติดต่อ' },
                    { en: 'Feedback', th: 'ข้อเสนอแนะ', hint: 'การตอบกลับ' },
                    { en: 'Meeting', th: 'การประชุม', hint: 'การพบปะ' },
                    { en: 'Report', th: 'รายงาน', hint: 'เอกสารสรุป' },
                    { en: 'Newsletter', th: 'จดหมายข่าว', hint: 'แจ้งข่าวสาร' },
                    { en: 'Transparency', th: 'ความโปร่งใส', hint: 'เปิดเผย' },
                    { en: 'Consensus', th: 'ฉันทามติ', hint: 'ความเห็นร่วมกัน' }
                ]
            },
            student: {
                'คำศัพท์พื้นฐาน': [
                    { en: 'School', th: 'โรงเรียน', hint: 'สถานที่เรียน' },
                    { en: 'Student', th: 'นักเรียน', hint: 'ผู้เรียน' },
                    { en: 'Teacher', th: 'ครู', hint: 'ผู้สอน' },
                    { en: 'Classroom', th: 'ห้องเรียน', hint: 'ห้องเรียน' },
                    { en: 'Book', th: 'หนังสือ', hint: 'ตำรา' },
                    { en: 'Homework', th: 'การบ้าน', hint: 'งานที่บ้าน' },
                    { en: 'Exam', th: 'สอบ', hint: 'การทดสอบ' },
                    { en: 'Grade', th: 'เกรด', hint: 'คะแนน' },
                    { en: 'Subject', th: 'วิชา', hint: 'รายวิชา' },
                    { en: 'Friend', th: 'เพื่อน', hint: 'คนคุ้นเคย' }
                ],
                'วิทยาศาสตร์': [
                    { en: 'Science', th: 'วิทยาศาสตร์', hint: 'ศาสตร์แห่งการค้นคว้า' },
                    { en: 'Experiment', th: 'การทดลอง', hint: 'การทดสอบ' },
                    { en: 'Biology', th: 'ชีววิทยา', hint: 'วิทยาศาสตร์ชีวิต' },
                    { en: 'Chemistry', th: 'เคมี', hint: 'วิทยาศาสตร์สารเคมี' },
                    { en: 'Physics', th: 'ฟิสิกส์', hint: 'วิทยาศาสตร์กายภาพ' },
                    { en: 'Hypothesis', th: 'สมมติฐาน', hint: 'การคาดเดา' },
                    { en: 'Observation', th: 'การสังเกต', hint: 'การดู' },
                    { en: 'Conclusion', th: 'สรุป', hint: 'ผลลัพธ์' },
                    { en: 'Laboratory', th: 'ห้องปฏิบัติการ', hint: 'ห้องทดลอง' },
                    { en: 'Microscope', th: 'กล้องจุลทรรศน์', hint: 'เครื่องมือดูสิ่งเล็ก' }
                ],
                'คณิตศาสตร์': [
                    { en: 'Mathematics', th: 'คณิตศาสตร์', hint: 'วิชาเลข' },
                    { en: 'Addition', th: 'การบวก', hint: '+' },
                    { en: 'Subtraction', th: 'การลบ', hint: '-' },
                    { en: 'Multiplication', th: 'การคูณ', hint: '×' },
                    { en: 'Division', th: 'การหาร', hint: '÷' },
                    { en: 'Fraction', th: 'เศษส่วน', hint: '½' },
                    { en: 'Decimal', th: 'ทศนิยม', hint: '0.5' },
                    { en: 'Geometry', th: 'เรขาคณิต', hint: 'รูปทรง' },
                    { en: 'Algebra', th: 'พีชคณิต', hint: 'x, y' },
                    { en: 'Equation', th: 'สมการ', hint: 'การเท่ากัน' }
                ],
                'สังคมศึกษา': [
                    { en: 'History', th: 'ประวัติศาสตร์', hint: 'อดีต' },
                    { en: 'Geography', th: 'ภูมิศาสตร์', hint: 'โลกและสถานที่' },
                    { en: 'Culture', th: 'วัฒนธรรม', hint: 'ประเพณี' },
                    { en: 'Society', th: 'สังคม', hint: 'ชุมชน' },
                    { en: 'Government', th: 'รัฐบาล', hint: 'การปกครอง' },
                    { en: 'Democracy', th: 'ประชาธิปไตย', hint: 'การปกครองระบบประชาชน' },
                    { en: 'Economy', th: 'เศรษฐกิจ', hint: 'การเงิน' },
                    { en: 'Citizen', th: 'พลเมือง', hint: 'ประชาชน' },
                    { en: 'Community', th: 'ชุมชน', hint: 'หมู่บ้าน' },
                    { en: 'Environment', th: 'สิ่งแวดล้อม', hint: 'ธรรมชาติรอบตัว' }
                ]
            }
        };

        let currentMode = '';
        let currentCategory = '';
        let currentCards = [];
        let currentIndex = 0;
        let isEnglishFirst = true;

        function selectMode(mode) {
            currentMode = mode;
            document.querySelectorAll('.mode-btn').forEach(btn => btn.classList.remove('active'));
            document.querySelector(`.mode-btn.${mode}`).classList.add('active');
            
            showCategories();
        }

        function showCategories() {
            document.getElementById('categorySection').classList.remove('hidden');
            document.getElementById('flashcardSection').classList.add('hidden');
            
            const categorySelector = document.getElementById('categorySelector');
            categorySelector.innerHTML = '';
            
            const categories = Object.keys(flashcards[currentMode]);
            categories.forEach(category => {
                const btn = document.createElement('button');
                btn.className = 'category-btn';
                btn.textContent = category;
                btn.onclick = () => selectCategory(category);
                categorySelector.appendChild(btn);
            });
        }

        function selectCategory(category) {
            currentCategory = category;
            currentCards = [...flashcards[currentMode][category]];
            currentIndex = 0;
            
            document.querySelectorAll('.category-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            
            document.getElementById('flashcardSection').classList.remove('hidden');
            displayCard();
        }

        function displayCard() {
            const card = currentCards[currentIndex];
            const flashcard = document.getElementById('flashcard');
            flashcard.classList.remove('flipped');
            
            if (isEnglishFirst) {
                document.getElementById('frontText').textContent = card.en;
                document.getElementById('backText').textContent = card.th;
            } else {
                document.getElementById('frontText').textContent = card.th;
                document.getElementById('backText').textContent = card.en;
            }
            
            updateProgress();
        }

        function flipCard() {
            document.getElementById('flashcard').classList.toggle('flipped');
        }

        function nextCard() {
            currentIndex = (currentIndex + 1) % currentCards.length;
            displayCard();
        }

        function previousCard() {
            currentIndex = (currentIndex - 1 + currentCards.length) % currentCards.length;
            displayCard();
        }

        function toggleLanguage() {
            isEnglishFirst = !isEnglishFirst;
            displayCard();
        }

        function shuffleCards() {
            for (let i = currentCards.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [currentCards[i], currentCards[j]] = [currentCards[j], currentCards[i]];
            }
            currentIndex = 0;
            displayCard();
        }

        function updateProgress() {
            document.getElementById('progress').textContent = 
                `การ์ดที่ ${currentIndex + 1} จาก ${currentCards.length}`;
        }

        // Keyboard navigation
        document.addEventListener('keydown', (e) => {
            if (document.getElementById('flashcardSection').classList.contains('hidden')) return;
            
            if (e.key === 'ArrowLeft') previousCard();
            if (e.key === 'ArrowRight') nextCard();
            if (e.key === ' ') {
                e.preventDefault();
                flipCard();
            }
        });
    </script>
</body>
</html>
