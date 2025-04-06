<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>منصة العلوم المتكاملة</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }
        body {
            background: linear-gradient(45deg, #1a365f, #2a5298);
            min-height: 100vh;
            color: white;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        .subjects-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            padding: 40px 0;
        }
        .subject-card {
            background: rgba(255,255,255,0.1);
            padding: 20px;
            border-radius: 12px;
            cursor: pointer;
            transition: 0.3s;
        }
        .subject-card:hover {
            transform: translateY(-5px);
            background: rgba(116, 185, 255, 0.2);
        }
        .quiz-container {
            background: rgba(255,255,255,0.1);
            padding: 30px;
            border-radius: 15px;
            margin: 20px 0;
        }
        .question {
            margin: 20px 0;
            padding: 15px;
            background: rgba(0,0,0,0.2);
            border-radius: 8px;
        }
        .options label {
            display: block;
            margin: 10px 0;
            padding: 10px;
            background: rgba(255,255,255,0.05);
            border-radius: 5px;
            cursor: pointer;
        }
        .result-box {
            text-align: center;
            padding: 20px;
            background: rgba(0,255,0,0.1);
            border-radius: 10px;
            margin: 20px 0;
        }
        button {
            padding: 10px 30px;
            background: #74b9ff;
            border: none;
            border-radius: 5px;
            color: white;
            cursor: pointer;
            transition: 0.3s;
            margin: 10px;
        }
        button:hover {
            background: #0984e3;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 style="text-align: center; margin: 30px 0;">🔬 العلوم المتكاملة - التقييم السادس </h1>
        <div class="subjects-grid" id="subjectsList">
            <div class="subject-card" onclick="startExam('علوم')">🔬 العلوم المتكاملة</div>
        </div>
        <div id="quizSection" style="display: none;">
            <button onclick="backToSubjects()">← العودة</button>
            <div id="quizContent"></div>
            <div class="result-box" id="result"></div>
        </div>
    </div>
    <script>
        const questionBank = {
            'علوم': [
                {
                    question: "ما المادة الأساسية المستخدمة في تصنيع الخلايا الشمسية؟",
                    options: ["النحاس", "السيليكون", "الألومنيوم", "الحديد"],
                    answer: 1
                },
                {
                    question: "كيف تعمل الخلايا الشمسية على تحويل الطاقة الضوئية إلى طاقة كهربائية؟",
                    options: [
                        "عن طريق تسخين المياه",
                        "عن طريق إزاحة الإلكترونات بواسطة الفوتونات",
                        "عن طريق حرق الوقود الأحفوري",
                        "عن طريق تحريك التوربينات"
                    ],
                    answer: 1
                },
                {
                    question: "كيف يمكن قياس كفاءة الخلايا الشمسية؟",
                    options: [
                        "بمقارنة الطاقة الكهربائية الناتجة بالطاقة الضوئية الساقطة",
                        "بقياس درجة حرارة الخلية",
                        "بقياس وزن الخلية",
                        "بقياس سرعة دوران الخلية"
                    ],
                    answer: 0
                },
                {
                    question: "إذا كانت خلية شمسية تنتج فرق جهد 12 فولت وتمرر تيارًا شدته 2 أمبير، فما القدرة الكهربائية الناتجة؟",
                    options: ["6 وات", "12 وات", "24 وات", "48 وات"],
                    answer: 2
                },
                {
                    question: "لماذا تُبنى توربينات الرياح في المناطق المفتوحة والمرتفعة؟",
                    options: [
                        "لأنها أقل تكلفة في هذه المناطق",
                        "لأن سرعة الرياح تكون أعلى في هذه المناطق",
                        "لأنها مناطق سياحية",
                        "لأنها قريبة من المدن"
                    ],
                    answer: 1
                },
                {
                    question: "إذا كانت كفاءة خلية شمسية 20%، وكانت القدرة الضوئية الساقطة عليها 500 وات، فما هي القدرة الكهربائية الناتجة؟",
                    options: ["50 وات", "100 وات", "200 وات", "250 وات"],
                    answer: 1
                },
                {
                    question: "ما الفائدة الرئيسية من استخدام الطاقة المتجددة مقارنة بالوقود الأحفوري؟",
                    options: [
                        "زيادة التلوث",
                        "تقليل انبعاثات الكربون",
                        "ارتفاع التكلفة",
                        "الاعتماد على الموارد غير المتجددة"
                    ],
                    answer: 1
                },
                {
                    question: "ما التحديات الرئيسية التي تواجه استخدام الطاقة الشمسية في المناطق الصحراوية؟",
                    options: [
                        "قلة أشعة الشمس",
                        "تراكم الأتربة على الألواح الشمسية",
                        "انخفاض تكلفة التركيب",
                        "عدم وجود تقنيات حديثة"
                    ],
                    answer: 1
                },
                {
                    question: "إذا كانت القدرة الضوئية الساقطة على خلية شمسية تساوي 1000 وات، والقدرة الكهربائية الناتجة تساوي 250 وات، فإن كفاءة الخلية تساوي...........",
                    options: ["10%", "15%", "20%", "25%"],
                    answer: 3
                },
                {
                    question: "لماذا تُعتبر الطاقة الكهرومائية صديقة للبيئة مقارنة بالوقود الأحفوري؟",
                    options: [
                        "لأنها تنتج انبعاثات كربونية عالية",
                        "لأنها تعتمد على حركة المياه ولا تنتج انبعاثات ضارة",
                        "لأنها تتطلب حرق الوقود",
                        "لأنها غير متجددة"
                    ],
                    answer: 1
                },
                {
                    question: "ما هي أبرز التحديات التي تواجه استخدام طاقة الرياح في المناطق الحضرية؟",
                    options: [
                        "قلة سرعة الرياح",
                        "التكلفة العالية للتركيب",
                        "الحاجة إلى مساحات كبيرة",
                        "جميع ما سبق"
                    ],
                    answer: 3
                },
                {
                    question: "ما الفوائد البيئية لاستخدام الطاقة الشمسية؟",
                    options: [
                        "زيادة انبعاثات الكربون",
                        "تقليل الاعتماد على الوقود الأحفوري",
                        "زيادة التلوث",
                        "استنفاذ الموارد الطبيعية"
                    ],
                    answer: 1
                }
            ]
        };

        let currentSubject = '';
        let userAnswers = [];

        function startExam(subject) {
            currentSubject = subject;
            document.getElementById('subjectsList').style.display = 'none';
            document.getElementById('quizSection').style.display = 'block';
            loadQuestions();
        }

        function loadQuestions() {
            const quizContent = document.getElementById('quizContent');
            quizContent.innerHTML = '';
            userAnswers = [];
            
            questionBank[currentSubject].forEach((q, index) => {
                quizContent.innerHTML += `
                    <div class="quiz-container">
                        <div class="question">السؤال ${index+1}: ${q.question}</div>
                        <div class="options">
                            ${q.options.map((opt, i) => `
                                <label>
                                    <input type="radio" name="q${index}" value="${i}">
                                    ${opt}
                                </label>
                            `).join('')}
                        </div>
                    </div>
                `;
            });
            
            quizContent.innerHTML += `<button onclick="submitQuiz()">تقديم الإجابات</button>`;
        }

        function submitQuiz() {
            const questions = document.querySelectorAll('.quiz-container');
            let correct = 0;
            
            questions.forEach((q, index) => {
                const selected = q.querySelector('input:checked');
                const questionData = questionBank[currentSubject][index];
                
                if(selected && parseInt(selected.value) === questionData.answer) {
                    correct++;
                }
            });
            
            const totalQuestions = questions.length;
            const percentage = ((correct / totalQuestions) * 100).toFixed(1);
            
            document.getElementById('result').innerHTML = `
                <h3>🎉 النتيجة النهائية</h3>
                <p>عدد الإجابات الصحيحة: ${correct}</p>
                <p>عدد الإجابات الخاطئة: ${totalQuestions - correct}</p>
                <p>النسبة المئوية: ${percentage}%</p>
                <button onclick="loadQuestions()">إعادة الاختبار</button>
            `;
        }

        function backToSubjects() {
            document.getElementById('subjectsList').style.display = 'grid';
            document.getElementById('quizSection').style.display = 'none';
        }
    </script>
</body>
</html>
