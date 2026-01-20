
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
                <i class="fas fa-book-open mr-2 text-indigo-600"></i> คำภีร์พิชัยสงครามฉบับเกมMOBA
            </h1>
            <p class="text-gray-600">ค้นหาคำศัพท์เกี่ยวกับเกมMOBAได้อย่างรวดเร็ว!!!</p>
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
        
        const dictionaryData = {
             "invade": {
                meaning: "ป่วนป่า - การไปรุกรามพื้นที่ฟาร์มป่าของฝั้งตรงข้าม",
                type: "invade/vade/อินเวด/เวด/เหวด (คำกริยา)",
                example: "Krixi!!! เธอไปvadeหน่อย!!."
            },
            "vade": {
                meaning: "ป่วนป่า - การไปรุกรามพื้นที่ฟาร์มป่าของฝั้งตรงข้าม",
                type: "invade/vade/อินเวด/เวด/เหวด (คำกริยา)",
                example: "Krixi!!! เธอไปInvadeหน่อย!!."
            },
            "อินเวด": {
                meaning: "ป่วนป่า - การไปรุกรามพื้นที่ฟาร์มป่าของฝั้งตรงข้าม",
                type: "invade/vade/อินเวด/เวด/เหวด (คำกริยา)",
                example: "Krixi!!! เธอไปอินเวดหน่อย!!."
            },
            "เวด": {
                meaning: "ป่วนป่า - การไปรุกรามพื้นที่ฟาร์มป่าของฝั้งตรงข้าม",
                type: "invade/vade/อินเวด/เวด/เหวด (คำกริยา)",
                example: "Krixi!!! เธอไปเวดหน่อย!!."
            },
            "เหวด": {
                meaning: "ป่วนป่า - การไปรุกรามพื้นที่ฟาร์มป่าของฝั้งตรงข้าม",
                type: "invade/vade/อินเวด/เวด/เหวด (คำกริยา)",
                example: "Krixi!!! เธอไปเหวดหน่อย!!."
            },
            "ปรานอม": {
                meaning: "แม่ตอง - แม่ปรานอมห้าดาวขนมไทย",
                type: "แม่ตอง (คำนาม)",
                example: "Krixi!!! เธอไปInvadeหน่อย!!."
            },
            "สมชาย": {
                meaning: "พ่อตอง - พ่อสมชายขับscoopyiมารับไอ่ตอง",
                type: "พ่อตอง (คำนาม)",
                example: "Krixi!!! เธอไปInvadeหน่อย!!."
            },
            "จบบน": {
                meaning: "จบบน - ตำแหน่งป่าฝั่งตรงข้ามฟามจบที่เลนบน",
                type: "จบบน (คำกริยา)",
                example: "ฮีโร่ป่าฝั่งตรงข้ามจบบน."
            },
            "จบล่าง": {
                meaning: "จบล่าง - ตำแหน่งป่าฝั่งตรงข้ามฟามจบที่เลนบน ",
                type: "จบล่าง (คำกริยา)",
                example: "ฮีโร่ป่าฝั่งตรงข้ามจบจบล่าง."
            },
            "ขวย": {
                meaning: "ขวย - ควย ",
                type: "ขวย (คำนาม)",
                example: "ขวย!!."
            },
            "กิฟ": {
                meaning: "กิฟ - แม่ไอ่ตุลย์ ",
                type: "กิฟ/กิ๊ฟ/gift (คำนาม)",
                example: "."
            },
            "กิ๊ฟ": {
                meaning: "กิ๊ฟ - แม่ไอ่ตุลย์ ",
                type: "กิฟ/กิ๊ฟ/gift (คำนาม)",
                example: "."
            },
            "gift": {
                meaning: "gift - แม่ไอ่ตุลย์ ",
                type: "กิฟ/กิ๊ฟ/gift (คำนาม)",
                example: "."
            },
            "ต้อม": {
                meaning: "ต้อม - พ่อนนท์ ",
                type: "ต้อม (คำนาม)",
                example: "."
            },
            "ตู่": {
                meaning: "ตู่ - พ่อต้นบุญ ",
                type: "ตู๋ (คำนาม)",
                example: "."
            },
            "วิชัย": {
                meaning: "วิชัย - พ่อหวาย ",
                type: " (คำนาม)",
                example: "."
            },
            "ปอมปอม": {
                meaning: "ปอมปอม - พ่อนมัส ",
                type: "ปอม/ปอมปอม/pom (คำนาม)",
                example: "."
            },
            "ปอม": {
                meaning: "ปอม - พ่อนมัส ",
                type: "ปอม/ปอมปอม/pom (คำนาม)",
                example: "."
            },
            "pom": {
                meaning: "pom - พ่อนมัส ",
                type: "ปอม/ปอมปอม/pom (คำนาม)",
                example: "."
            },
            "ดราฟ": {
                meaning: "ดราฟ - การเลือกตัวละครไปสู้กับทีมตรงข้ามหรือการวางแผนในการเอาตัวละครไปใช้ ",
                type: "ดราฟ (คำกริยา)",
                example: "ดราฟฟลอเรนมาแก้โตโร้นะ!!."
            },
            "ไดร์ฟ": {
                meaning: "ไดร์ฟ - การที่ทีมเราไปรุมตัวละครของฝั่งตรงข้ามที่มีน้อยกว่าทีมเรา ",
                type: "ไดร์ฟ/โถม/drive (คำกริยา)",
                example: "ไดร์ฟเเครี่มันเลยเพื่อน!!."
            },
            "โถม": {
                meaning: "โถม - การที่ทีมเราไปรุมตัวละครของฝั่งตรงข้ามที่มีน้อยกว่าทีมเรา ",
                type: "ไดร์ฟ/โถม/drive (คำกริยา)",
                example: "โถมเเครี่มันเลยเพื่อน!!."
            },
            "drive": {
                meaning: "drive - การที่ทีมเราไปรุมตัวละครของฝั่งตรงข้ามที่มีน้อยกว่าทีมเรา ",
                type: "ไดร์ฟ/โถม/drive (คำกริยา)",
                example: "Driveเเครี่มันเลยเพื่อน!!."
            },
            "DPS": {
                meaning: "DPS - Damage per second ดาเมจต่อวินาทีที่ทำได้ ",
                type: "DPS/ดีพีเอส (คำนาม)",
                example: "หยิบแครี่DPSสูงๆมา!!."
            },
            "ดีพีเอส": {
                meaning: "DPS - Damage per second ดาเมจต่อวินาทีที่ทำได้ ",
                type: "DPS/ดีพีเอส (คำนาม)",
                example: "หยิบแครี่ดีพีเอสสูงๆมา!!."
            },
            "cc": {
                meaning: "cc - สกิวที่ทำให้ทีมตรงข้ามโดรสตั้นหรือไม่สามารถทำอะไรได้ ",
                type: "cc/ซีซี (คำนาม)",
                example: "เข้าไปทำccให้หน่อยเพื่อน!!."
            },
            "ซีซี": {
                meaning: "cc - สกิวที่ทำให้ทีมตรงข้ามโดรสตั้นหรือไม่สามารถทำอะไรได้ ",
                type: "cc/ซีซี (คำนาม)",
                example: "เข้าไปทำซีซีให้หน่อยเพื่อน!!."
            },
            "Bait": {
                meaning: "Bait - การล่อทีมตรงข้ามให้ตามมา ",
                type: "Bait/เบท (คำกริยา)",
                example: "ไปBaitให้หน่อยเพื่อน!!."
            },
            "เบท": {
                meaning: "เบท - การล่อทีมตรงข้ามให้ตามมา ",
                type: "Bait/เบท (คำกริยา)",
                example: "ไปเบทให้หน่อยเพื่อน!!."
            },
            "ล้าง": {
                meaning: "ล้าง - การล้างCCหรือล้างสถานะผิดปกติให้เพื่อน ",
                type: "ล้าง (คำกริยา)",
                example: "ล้างให้หน่อย/ออกล้างให้หน่อย ccเขาเยอะ!!."
            },
            "ตัดเลือด": {
                meaning: "ตัดเลือด - ของที่สามารถลดค่าการฮีลเลือดของทีมตรงข้ามได้ ",
                type: "ตัดเลือด (คำนาม)",
                example: "ออกตัดเลือดหน่อย!!."
            },
            "ฮีล": {
                meaning: "ฮีล - การรีเลือดทำให้พลังชีวิตของตัวเองฟื้นฟู ",
                type: "ฮีล/Heal (คำกริยา)",
                example: "ฮีลให้หน่อยหรือออกของฮีลให้หน่อย!!."
            },
            "Heal": {
                meaning: "ฮีล - การรีเลือดทำให้พลังชีวิตของตัวเองฟื้นฟู ",
                type: "ฮีล/Heal (คำกริยา)",
                example: "Healให้หน่อยหรือออกของฮีลให้หน่อย!!."
            },
            "ของโรม": {
                meaning: "ของโรม - ของที่ตำแหน่งโรมมิ่งออกมาช่วยทีมซึ่งมีหลายชนิดเช่นล้างเปิดแมพฮีลหรือเกราะซึ่งของโรมจะมีสามารถพิเศษคือไม่แย่งเงินจากเพื่อนในทีม ",
                type: "ของโรม (คำนาม)",
                example: "ออกของโรมมานะออกล้าง,ฮีล,เปิดแมพ,เกราะ!!."
            },
            "Split lane": {
                meaning: "Split lane - แยกไปเลนตรงข้ามหรือไปดันเลนตรงข้ามที่เพื่อนอยู่ ",
                type: "Split lane (คำกริยา)",
                example: "ออฟเลนSplit laneหน่อย!!."
            },
            "Freeze wave": {
                meaning: "Freeze wave - แช่เลนหรือการทำให้ครีปตีกันโดยที่เราไม่เครียร์ครีป ",
                type: "Freeze wave/แช่เลน (คำกริยา)",
                example: "Freeze waveไว้รอให้ครีปเข้าพร้อมกัน!!."
            },
            "แช่เลน": {
                meaning: "แช่เลน - แช่เลนหรือการทำให้ครีปตีกันโดยที่เราไม่เครียร์ครีป ",
                type: "Freeze wave/แช่เลน (คำกริยา)",
                example: "แช่เลนไว้รอให้ครีปเข้าพร้อมกัน!!."
            },
            "Ultimate": {
                meaning: "Ultimate - สกิลอันติหรือท่าไม้ตาย ",
                type: "Ultimate/ULT/อันติ/อันติเมต (คำกริยา)",
                example: "Ultimateพร้อมเเล้วเข้าได้เลย,ฝั่งตรงข้ามUltเเล้ว!!."
            },
            "Ult": {
                meaning: "Ulti - สกิลอันติหรือท่าไม้ตาย ",
                type: "Ultimate/ULT/อันติ/อันติเมต (คำกริยา)",
                example: "Ultพร้อมเเล้วเข้าได้เลย,ฝั่งตรงข้ามUltเเล้ว!!."
            },
            "อันติ": {
                meaning: "อันติ - สกิลอันติหรือท่าไม้ตาย ",
                type: "Ultimate/ULT/อันติ/อันติเมต (คำกริยา)",
                example: "อันติพร้อมเเล้วเข้าได้เลย,ฝั่งตรงข้ามUltเเล้ว!!."
            },
            "อันติเมต": {
                meaning: "อันติเมต - สกิลอันติหรือท่าไม้ตาย ",
                type: "Ultimate/ULT/อันติ/อันติเมต (คำกริยา)",
                example: "อันติเมตพร้อมเเล้วเข้าได้เลย,ฝั่งตรงข้ามUltเเล้ว!!."
            },
            "ครีป": {
                meaning: "ครีป - เป็นมีนเนียนที่ป้อมใหญ่เราสร้างขึ้นมาและเมื่อเราตีครีปจนตายเราจะได้เงิน ",
                type: "ครีป/Creep (คำนาม)",
                example: "รีบๆเครียร์ครีปแล้วมาเติมเลน!!."
            },
            "Creep": {
                meaning: "Creep - เป็นมีนเนียนที่ป้อมใหญ่เราสร้างขึ้นมาและเมื่อเราตีครีปจนตายเราจะได้เงิน ",
                type: "ครีป/Creep (คำนา่ม)",
                example: "รีบๆเครียร์Creepแล้วมาเติมเลน!!."
            },
            "ป้อม": {
                meaning: "ป้อม - ป้อมคือฐานของทั้งสองทีมซึ่งมีป้อมใหญ่กับป้อมเล็กซึ่งแต่ละทีมจะมีอยู่สิบป้อม ",
                type: "ป้อม (คำนาม)",
                example: "กันป้อมไว้,ดันป้อมแล้วมาเลนอื่นต่อ!!."
            },
            "CD": {
                meaning: "CD - สกิลติดคลูดาวน์คือสกิจไม่พร้อมใช้งานต้องรอเวลา ",
                type: "CD (คำกริยา)",
                example: "สกิลCDอยู่อย่าพึ่งเข้า!!."
            },
            "NF": {
                meaning: "NF - ฝั่งตรงข้ามหรือฝั่งเราไม่มีฟิกเกอร์ ",
                type: "NF (คำกริยา)",
                example: "ฝั่งตรงข้ามNFเข้าได้เลย!!."
            },
            "ออปเจ็กต์": {
                meaning: "ออปเจ็กต์ - สิ่งที่ให้บัฟกับทีมเรา&ทีมตรงข้ามเมื่อเราฆ่าเสร็จซึ่งมีAbyssal dragon&Dark slayer ",
                type: "ออปเจ็กต์/objects/object (คำนาม)",
                example: "มาตีออปเจ็กต์!!."
            },
            "objects": {
                meaning: "objects - สิ่งที่ให้บัฟกับทีมเรา&ทีมตรงข้ามเมื่อเราฆ่าเสร็จซึ่งมีAbyssal dragon&Dark slayer ",
                type: "ออปเจ็กต์/objects/object (คำนาม)",
                example: "มาตีobjects!!."
            },
            "object": {
                meaning: "object - สิ่งที่ให้บัฟกับทีมเรา&ทีมตรงข้ามเมื่อเราฆ่าเสร็จซึ่งมีAbyssal dragon&Dark slayer ",
                type: "ออปเจ็กต์/objects/object (คำนาม)",
                example: "มาตีobject!!."
            },
            "โซน": {
                meaning: "โซน - การกันหรือไปดูศัตรูที่กำลังจะเข้ามาหาทีมเรา ",
                type: "โซน/zone (คำกริยา)",
                example: "โรมโซนออปเจ็กต์หน่อย!!."
            },
            "zone": {
                meaning: "zone - การกันหรือไปดูศัตรูที่กำลังจะเข้ามาหาทีมเรา ",
                type: "โซน/zone (คำกริยา)",
                example: "โรมzoneออปเจ็กต์หน่อย!!."
            },
            "SM": {
                meaning: "SM - เป็นการเตือนเพื่อนให้ดูเเผนที่หรือให้โรมไปเปิดแผนที่ ",
                type: "SM (คำกริยา)",
                example: "โรมSMหน่อย!!."
            },
            "Abyssal dragon": {
                meaning: "Abyssal dragon - เป็นออปเจ็กต์ตอนเรากำจัดมันได้ทีมเราจะได้บัฟดาเมจเวทมา ",
                type: "Abyssal dragon (คำนาม)",
                example: "ตีAbysal dragonหน่อย!!."
            },
            "VC": {
                meaning: "VC - ให้เพื่อนเปิดไมค์คลอเกม ",
                type: "VC (คำนาม)",
                example: "ทีมเปิดVCจะได้บอกข้อมูลได้!!."
            },
            "Dark Slayer": {
                meaning: "Dark Slayer - ออปเจ็กต์มีรูปร่างเป็นมังกรตัวสีม่วงถ้าเรากำจัดมันได้เราจะได้บัฟในการดันป้อมมาคือเสกVanguard ",
                type: "Dark Slayer (คำนาม)",
                example: "เปิดDark slayerหน่อย!!."
            },
            "Vanguard": {
                meaning: "Vanguard - เป็นมังกรตัวเล็กๆสีม่วงได้มาจากDark slayer ",
                type: "Vanguard (คำนาม)",
                example: "เคลียร์Vanguardหน่อย!!."
            },
            "RP": {
                meaning: "RP - เป็นการบอกให้เพื่อนรายงานคนที่น่าสงสัย ",
                type: "RP (คำกริยา)",
                example: "เพื่อนRPป่าฝั่งตรงข้ามที่!!."
            },
            
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
            
            const utterance = new SpeechSynthesisUtterance(word);
            utterance.lang = 'en-US';
            window.speechSynthesis.speak(utterance);
        }
    </script>

<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>คำศัพท์</title>
    <style>
        table {
            width: 100%;
            border-collapse: collapse;
            font-family: sans-serif;
        }

        th, td {
            border: 1px solid #000;
            padding: 10px;
            text-align: left;
        }

        /* ปรับสัดส่วนตามที่คุณต้องการ */
        .col-medium {
            width: 25%;  /* กลาง */
            background-color: #f9f9f9;
        }
        .col-short {
            width: 10%;   /* สั้น */
            text-align: center;
        }
        .col-very-long {
            width: 65%;  /* ยาวมาก */
        }

        thead {
            background-color: #e0e0e0;
        }
    </style>
</head>
<body>

    <h2>คำศัพท์</h2>

    <table>
        <thead>
            <tr>
                <th class="col-medium">ชิ่อคำ</th>
                <th class="col-short">ชนิดของคำ</th>
                <th class="col-very-long">ความหมาย</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>ชิ่อคำ</td>
                <td>ชนิด</td>
                <td>ความหมาย</td>
            </tr>
            <tr>
                <td>ชิ่อคำ</td>
                <td>ชนิด</td>
                <td>ความหมาย</td>
            </tr>
            </tbody>
    </table>

</body>
</html>
</body>
</html>
