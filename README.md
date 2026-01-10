
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>คำภีร์พิชัยสงครามฉบับเกมMOBA</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Kanit', sans-serif; }
        .glass-morphism {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
        }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-50 to-indigo-100 min-h-screen">

    <div class="container mx-auto px-4 py-12 max-w-2xl">
        <div class="text-center mb-10">
            <h1 class="text-4xl font-bold text-indigo-900 mb-2">
                <i class="fas fa-book-open mr-2 text-indigo-600"></i> My Dictionary
            </h1>
            <p class="text-gray-600">ค้นหาคำศัพท์เกี่ยวกับเกมMOBAได้อย่างรวดเร็ว</p>
        </div>

        <div class="glass-morphism rounded-2xl shadow-xl p-2 flex items-center mb-8 border border-white">
            <input type="text" id="searchInput" 
                placeholder="พิมพ์คำศัพท์ที่เจ้าต้องการค้นหา..." 
                class="w-full p-4 rounded-xl focus:outline-none text-lg bg-transparent"
                onkeyup="handleSearch(event)">
            <button onclick="searchWord()" 
                class="bg-indigo-600 hover:bg-indigo-700 text-white px-8 py-4 rounded-xl transition duration-300 font-medium">
                <i class="fas fa-search"></i>
            </button>
        </div>

        <div id="resultArea" class="hidden">
            <div class="bg-white rounded-3xl shadow-lg p-8 transform transition duration-500 hover:scale-[1.01]">
                <div class="flex justify-between items-start mb-6">
                    <div>
                        <h2 id="wordTitle" class="text-4xl font-bold text-indigo-900 lowercase capitalize">Word</h2>
                        <span id="wordType" class="inline-block mt-2 px-3 py-1 bg-indigo-100 text-indigo-700 rounded-full text-sm font-medium italic">noun</span>
                    </div>
                    <button onclick="playAudio()" class="text-indigo-500 hover:text-indigo-700 text-2xl p-3 bg-indigo-50 rounded-full transition">
                        <i class="fas fa-volume-up"></i>
                    </button>
                </div>

                <div class="space-y-4">
                    <div>
                        <h3 class="text-gray-400 text-sm uppercase tracking-wider font-semibold">ความหมาย</h3>
                        <p id="wordMeaning" class="text-xl text-gray-800 leading-relaxed">คำแปลจะปรากฏตรงนี้</p>
                    </div>
                    
                    <div class="pt-4 border-t border-gray-100">
                        <h3 class="text-gray-400 text-sm uppercase tracking-wider font-semibold mb-2">ตัวอย่างประโยค</h3>
                        <p id="wordExample" class="text-gray-600 italic">"This is an example sentence."</p>
                    </div>
                </div>
            </div>
        </div>

        <div id="notFound" class="hidden text-center py-10">
            <i class="fas fa-search text-gray-300 text-5xl mb-4"></i>
            <p class="text-gray-500">ไม่พบคำศัพท์ที่คุณค้นหา ลองใหม่อีกครั้ง</p>
        </div>
    </div>

    <script>
        // ข้อมูลจำลอง (Mock Data) - ในอนาคตสามารถเชื่อมต่อ API ได้
        const dictionaryData = {
            "apple": {
                meaning: "แอปเปิล (ผลไม้ชนิดหนึ่ง)",
                type: "noun",
                example: "She is eating a red apple."
            },
            "code": {
                meaning: "รหัส, การเขียนโปรแกรม",
                type: "noun / verb",
                example: "I love writing clean code."
            },
            "happy": {
                meaning: "มีความสุข, ยินดี",
                type: "adjective",
                example: "They look so happy together."
            }
        };

        function handleSearch(event) {
            if (event.key === "Enter") {
                searchWord();
            }
        }

        function searchWord() {
            const input = document.getElementById('searchInput').value.toLowerCase().trim();
            const resultArea = document.getElementById('resultArea');
            const notFound = document.getElementById('notFound');

            if (dictionaryData[input]) {
                document.getElementById('wordTitle').innerText = input;
                document.getElementById('wordType').innerText = dictionaryData[input].type;
                document.getElementById('wordMeaning').innerText = dictionaryData[input].meaning;
                document.getElementById('wordExample').innerText = `"${dictionaryData[input].example}"`;
                
                resultArea.classList.remove('hidden');
                notFound.classList.add('hidden');
            } else {
                resultArea.classList.add('hidden');
                notFound.classList.remove('hidden');
            }
        }

        function playAudio() {
            const word = document.getElementById('wordTitle').innerText;
            // ใช้ Web Speech API เพื่ออ่านออกเสียง
            const utterance = new SpeechSynthesisUtterance(word);
            utterance.lang = 'en-US';
            window.speechSynthesis.speak(utterance);
        }
    </script>
</body>
</html>
