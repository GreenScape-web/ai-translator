<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مترجم اللغات</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0d1117;
            color: #c9d1d9;
            direction: rtl;
        }
        .text-area-container {
            position: relative;
        }
        .text-area-container textarea {
            padding-right: 2.5rem;
            /* إضافة بعض الظل والتحسينات للمظهر */
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
            border-radius: 1rem;
        }
        .microphone-icon {
            position: absolute;
            top: 1.1rem;
            left: 1rem;
            color: #4b5563;
            cursor: pointer;
            transition: color 0.2s;
        }
        .speaker-icon {
            position: absolute;
            top: 1.1rem;
            left: 1rem;
            color: #4b5563;
            cursor: pointer;
            transition: color 0.2s;
        }
        .language-picker {
            position: relative;
        }
        .language-dropdown {
            position: absolute;
            top: 100%;
            right: 0;
            width: 100%;
            max-height: 250px;
            overflow-y: auto;
            background-color: #1f2937;
            border: 1px solid #4b5563;
            border-radius: 0.5rem;
            z-index: 10;
            margin-top: 0.5rem;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .language-dropdown input {
            direction: ltr;
        }
        .language-item {
            padding: 0.75rem 1rem;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        .language-item:hover {
            background-color: #374151;
        }
    </style>
</head>
<body class="flex flex-col min-h-screen">
    <!-- Header Section -->
    <header class="bg-gray-900 py-4 px-8 border-b border-gray-700">
        <div class="container mx-auto flex justify-between items-center">
            <h1 class="text-xl sm:text-2xl font-bold text-indigo-400">مترجمك الخاص</h1>
            <nav>
                <a href="#" class="text-gray-400 hover:text-indigo-400 transition-colors duration-200 text-sm sm:text-base">الرئيسية</a>
            </nav>
        </div>
    </header>

    <!-- Main Content Area -->
    <main class="flex-grow flex items-center justify-center p-4">
        <div class="w-full max-w-5xl p-6 md:p-12 bg-gray-800 rounded-3xl shadow-2xl border border-gray-700">
            <h2 class="text-2xl sm:text-3xl font-bold text-center mb-4 text-indigo-400">ابدأ الترجمة الآن</h2>
            <p class="text-center text-gray-400 mb-8 max-w-xl mx-auto">ترجم أي نص بسرعة وسهولة بين أكثر من 50 لغة.</p>

            <!-- Language and Swap Section -->
            <div class="flex items-center justify-center mb-6 gap-2">
                <!-- Source Language Picker -->
                <div id="source-lang-picker" class="relative w-1/2 language-picker">
                    <button id="source-lang-btn" class="w-full px-4 py-3 bg-gray-700 rounded-xl text-gray-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 flex justify-between items-center text-sm sm:text-base transition-colors duration-200 hover:bg-gray-600">
                        <span id="source-lang-name">العربية</span>
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 sm:h-5 sm:w-5 transform rotate-180" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
                        </svg>
                    </button>
                    <div id="source-lang-dropdown" class="language-dropdown hidden">
                        <input type="text" id="source-lang-search" placeholder="البحث عن لغة..." class="w-full px-4 py-2 bg-gray-900 border-b border-gray-700 rounded-t-xl text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-1 focus:ring-indigo-500">
                        <div id="source-lang-list"></div>
                    </div>
                    <input type="hidden" id="source-lang-value" value="ar">
                </div>

                <!-- Swap Button -->
                <button id="swap-btn" class="p-3 bg-gray-700 rounded-full hover:bg-gray-600 transition-all duration-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 transform hover:scale-110">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-indigo-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7h12m0 0l-4-4m4 4l-4 4m0 6H4m0 0l4 4m-4-4l4-4" />
                    </svg>
                </button>

                <!-- Target Language Picker -->
                <div id="target-lang-picker" class="relative w-1/2 language-picker">
                    <button id="target-lang-btn" class="w-full px-4 py-3 bg-gray-700 rounded-xl text-gray-300 focus:outline-none focus:ring-2 focus:ring-indigo-500 flex justify-between items-center text-sm sm:text-base transition-colors duration-200 hover:bg-gray-600">
                        <span id="target-lang-name">الإنجليزية</span>
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 sm:h-5 sm:w-5 transform rotate-180" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" />
                        </svg>
                    </button>
                    <div id="target-lang-dropdown" class="language-dropdown hidden">
                        <input type="text" id="target-lang-search" placeholder="البحث عن لغة..." class="w-full px-4 py-2 bg-gray-900 border-b border-gray-700 rounded-t-xl text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-1 focus:ring-indigo-500">
                        <div id="target-lang-list"></div>
                    </div>
                    <input type="hidden" id="target-lang-value" value="en">
                </div>
            </div>

            <!-- Text Areas -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="flex flex-col text-area-container">
                    <textarea id="source-text" rows="10" class="w-full p-4 bg-gray-900 border border-gray-700 rounded-xl text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none" placeholder="أدخل النص هنا..."></textarea>
                    <!-- أيقونة الميكروفون -->
                    <svg id="source-mic-icon" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 microphone-icon" viewBox="0 0 24 24" fill="currentColor">
                        <path d="M12 2A4 4 0 008 6v4a4 4 0 008 0V6a4 4 0 00-4-4zM7 11v1a5 5 0 005 5h.1A5 5 0 0017 12v-1h2v1a7 7 0 01-7 7v4h-2v-4a7 7 0 01-7-7v-1h2z"/>
                    </svg>
                </div>

                <div class="flex flex-col text-area-container">
                    <textarea id="target-text" rows="10" class="w-full p-4 bg-gray-900 border border-gray-700 rounded-xl text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none" placeholder="النص المترجم سيظهر هنا..." readonly></textarea>
                    <!-- أيقونة مكبر الصوت -->
                    <svg id="target-speaker-icon" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 speaker-icon hidden" viewBox="0 0 24 24" fill="currentColor">
                        <path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.74 2.5-2.26 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.98 7-4.78 7-8.77s-2.99-7.79-7-8.77z"/>
                    </svg>
                </div>
            </div>

            <!-- Translate Button and Loading Indicator -->
            <div class="flex flex-col sm:flex-row items-center justify-center mt-8 gap-4">
                <button id="translate-btn" class="w-full sm:w-auto px-8 py-3 bg-indigo-500 hover:bg-indigo-600 text-white font-semibold rounded-full shadow-lg transition duration-300 transform hover:scale-105 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 focus:ring-offset-gray-800">
                    ترجم
                </button>
                <div id="loading-indicator" class="hidden flex items-center gap-2">
                    <div class="h-6 w-6 border-4 border-t-4 border-indigo-400 border-solid rounded-full animate-spin"></div>
                    <span class="text-gray-400">جارٍ الترجمة...</span>
                </div>
            </div>
            
            <div id="message-box" class="mt-6 p-4 text-center rounded-lg hidden"></div>
        </div>
    </main>

    <!-- Footer Section -->
    <footer class="bg-gray-900 py-4 px-8 border-t border-gray-700 text-center text-gray-500 text-sm">
        <p>جميع الحقوق محفوظة © 2025</p>
    </footer>

    <script>
        // دالة لعرض الرسائل في صندوق مخصص بدلاً من alert
        function showMessage(message, type = 'info') {
            const box = document.getElementById('message-box');
            box.textContent = message;
            box.classList.remove('hidden', 'bg-red-500', 'bg-green-500', 'bg-blue-500');
            if (type === 'error') {
                box.classList.add('bg-red-500', 'text-white');
            } else if (type === 'success') {
                box.classList.add('bg-green-500', 'text-white');
            } else {
                box.classList.add('bg-blue-500', 'text-white');
            }
            setTimeout(() => {
                box.classList.add('hidden');
            }, 5000);
        }

        document.addEventListener('DOMContentLoaded', () => {
            const sourceText = document.getElementById('source-text');
            const targetText = document.getElementById('target-text');
            const sourceLangBtn = document.getElementById('source-lang-btn');
            const targetLangBtn = document.getElementById('target-lang-btn');
            const sourceLangName = document.getElementById('source-lang-name');
            const targetLangName = document.getElementById('target-lang-name');
            const sourceLangValue = document.getElementById('source-lang-value');
            const targetLangValue = document.getElementById('target-lang-value');
            const sourceLangDropdown = document.getElementById('source-lang-dropdown');
            const targetLangDropdown = document.getElementById('target-lang-dropdown');
            const sourceLangSearch = document.getElementById('source-lang-search');
            const targetLangSearch = document.getElementById('target-lang-search');
            const sourceLangList = document.getElementById('source-lang-list');
            const targetLangList = document.getElementById('target-lang-list');
            const translateBtn = document.getElementById('translate-btn');
            const swapBtn = document.getElementById('swap-btn');
            const loadingIndicator = document.getElementById('loading-indicator');
            const sourceMicIcon = document.getElementById('source-mic-icon');
            const targetSpeakerIcon = document.getElementById('target-speaker-icon');
            
            // Check for Web Speech API support
            const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
            if (!SpeechRecognition) {
                showMessage('متصفحك لا يدعم الإملاء الصوتي.', 'error');
                sourceMicIcon.style.display = 'none';
            }

            let recognition = null;

            // دالة لبدء التعرف على الكلام
            const startSpeechRecognition = (textArea, languageCode, icon) => {
                if (recognition) {
                    recognition.stop();
                }

                recognition = new SpeechRecognition();
                recognition.lang = languageCode;
                recognition.interimResults = false;
                recognition.maxAlternatives = 1;

                icon.style.color = '#34d399'; // تغيير لون الأيقونة إلى الأخضر للإشارة إلى الاستماع

                recognition.onresult = (event) => {
                    const speechResult = event.results[0][0].transcript;
                    textArea.value = speechResult;
                    icon.style.color = '#4b5563'; // إعادة اللون الأصلي
                    recognition = null;
                };

                recognition.onerror = (event) => {
                    console.error('خطأ في الإملاء الصوتي:', event.error);
                    showMessage(`خطأ في الإملاء الصوتي: ${event.error}`, 'error');
                    icon.style.color = '#4b5563'; // إعادة اللون الأصلي
                    recognition = null;
                };

                recognition.onend = () => {
                    icon.style.color = '#4b5563'; // إعادة اللون الأصلي عند الانتهاء
                    recognition = null;
                };

                recognition.start();
            };

            // إضافة مستمعي الأحداث لأيقونات الميكروفون
            sourceMicIcon.addEventListener('click', () => {
                const langCode = sourceLangValue.value;
                startSpeechRecognition(sourceText, langCode, sourceMicIcon);
            });

            // قائمة اللغات المدعومة
            const languages = [
                { code: 'ar', name: 'العربية' },
                { code: 'en', name: 'الإنجليزية' },
                { code: 'es', name: 'الإسبانية' },
                { code: 'fr', name: 'الفرنسية' },
                { code: 'de', name: 'الألمانية' },
                { code: 'zh', name: 'الصينية' },
                { code: 'ja', name: 'اليابانية' },
                { code: 'ko', name: 'الكورية' },
                { code: 'ru', name: 'الروسية' },
                { code: 'pt', name: 'البرتغالية' },
                { code: 'it', name: 'الإيطالية' },
                { code: 'tr', name: 'التركية' },
                { code: 'hi', name: 'الهندية' },
                { code: 'fa', name: 'الفارسية' },
                { code: 'ur', name: 'الأوردية' },
                { code: 'af', name: 'الأفريقانية' },
                { code: 'sq', name: 'الألبانية' },
                { code: 'am', name: 'الأمهرية' },
                { code: 'hy', name: 'الأرمينية' },
                { code: 'as', name: 'الآسامية' },
                { code: 'ay', name: 'الأيمارا' },
                { code: 'az', name: 'الأذربيجانية' },
                { code: 'eu', name: 'الباسكية' },
                { code: 'be', name: 'البيلاروسية' },
                { code: 'bn', name: 'البنغالية' },
                { code: 'bs', name: 'البوسنية' },
                { code: 'bg', name: 'البلغارية' },
                { code: 'ca', name: 'الكتالونية' },
                { code: 'ceb', name: 'السيبيونو' },
                { code: 'ny', name: 'النيانجا' },
                { code: 'co', name: 'الكورسيكية' },
                { code: 'hr', name: 'الكرواتية' },
                { code: 'cs', name: 'التشيكية' },
                { code: 'da', name: 'الدنماركية' },
                { code: 'nl', name: 'الهولندية' },
                { code: 'et', name: 'الإستونية' },
                { code: 'fi', name: 'الفنلندية' },
                { code: 'gl', name: 'الجاليكية' },
                { code: 'ka', name: 'الجورجية' },
                { code: 'el', name: 'اليونانية' },
                { code: 'gu', name: 'الغوجاراتية' },
                { code: 'ht', name: 'الكريول الهايتي' },
                { code: 'ha', name: 'الهوسا' },
                { code: 'haw', name: 'الهاوائية' },
                { code: 'iw', name: 'العبرية' },
                { code: 'hu', name: 'الهنغارية' },
                { code: 'is', name: 'الآيسلندية' },
                { code: 'ig', name: 'الإيغبو' },
                { code: 'id', name: 'الإندونيسية' },
                { code: 'ga', name: 'الأيرلندية' },
                { code: 'jw', name: 'الجاوية' },
                { code: 'kn', name: 'الكنادية' },
                { code: 'kk', name: 'الكازاخستانية' },
                { code: 'km', name: 'الخميرية' },
                { code: 'ky', name: 'القيرغيزية' },
                { code: 'lo', name: 'اللاوية' },
                { code: 'la', name: 'اللاتينية' },
                { code: 'lv', name: 'اللاتفية' },
                { code: 'lt', name: 'الليتوانية' },
                { code: 'mk', name: 'المقدونية' },
                { code: 'mg', name: 'المالاجاشية' },
                { code: 'ms', name: 'الماليزية' },
                { code: 'ml', name: 'الماليالامية' },
                { code: 'mt', name: 'المالطية' },
                { code: 'mi', name: 'الماورية' },
                { code: 'mr', name: 'الماراثية' },
                { code: 'mn', name: 'المنغولية' },
                { code: 'my', name: 'البورمية' },
                { code: 'ne', name: 'النيبالية' },
                { code: 'no', name: 'النرويجية' },
                { code: 'ps', name: 'البشتونية' },
                { code: 'pl', name: 'البولندية' },
                { code: 'ro', name: 'الرومانية' },
                { code: 'sm', name: 'الساموية' },
                { code: 'gd', name: 'الغيلية الأسكتلندية' },
                { code: 'sr', name: 'الصربية' },
                { code: 'st', name: 'السيسوتو' },
                { code: 'sn', name: 'الشونا' },
                { code: 'sd', name: 'السندية' },
                { code: 'si', name: 'السنهالية' },
                { code: 'sk', name: 'السلوفاكية' },
                { code: 'sl', name: 'السلوفينية' },
                { code: 'so', name: 'الصومالية' },
                { code: 'su', name: 'السوندانية' },
                { code: 'sw', name: 'السواحلية' },
                { code: 'sv', name: 'السويدية' },
                { code: 'tl', name: 'التاغالوغية' },
                { code: 'tg', name: 'الطاجيكية' },
                { code: 'ta', name: 'التاميلية' },
                { code: 'te', name: 'التيلوغوية' },
                { code: 'th', name: 'التايلاندية' },
                { code: 'uk', name: 'الأوكرانية' },
                { code: 'uz', name: 'الأوزبكية' },
                { code: 'vi', name: 'الفيتنامية' },
                { code: 'cy', name: 'الويلزية' },
                { code: 'xh', name: 'الخوسا' },
                { code: 'yi', name: 'اليديشية' },
                { code: 'yo', name: 'اليوروبا' },
                { code: 'zu', name: 'الزولو' }
            ];

            // دالة لملء قائمة اللغات
            const populateLanguageList = (listElement, filterValue = '', onSelectCallback) => {
                listElement.innerHTML = '';
                const filteredLanguages = languages.filter(lang => lang.name.includes(filterValue) || lang.code.includes(filterValue));
                filteredLanguages.forEach(lang => {
                    const div = document.createElement('div');
                    div.className = 'language-item text-right';
                    div.textContent = lang.name;
                    div.setAttribute('data-code', lang.code);
                    div.addEventListener('click', () => {
                        onSelectCallback(lang.code, lang.name);
                    });
                    listElement.appendChild(div);
                });
            };

            // تهيئة قوائم اللغات
            populateLanguageList(sourceLangList, '', (code, name) => {
                sourceLangValue.value = code;
                sourceLangName.textContent = name;
                sourceLangDropdown.classList.add('hidden');
            });
            populateLanguageList(targetLangList, '', (code, name) => {
                targetLangValue.value = code;
                targetLangName.textContent = name;
                targetLangDropdown.classList.add('hidden');
            });

            // فتح وإغلاق القوائم المنسدلة
            sourceLangBtn.addEventListener('click', () => {
                sourceLangDropdown.classList.toggle('hidden');
                targetLangDropdown.classList.add('hidden');
                sourceLangSearch.focus();
            });

            targetLangBtn.addEventListener('click', () => {
                targetLangDropdown.classList.toggle('hidden');
                sourceLangDropdown.classList.add('hidden');
                targetLangSearch.focus();
            });

            // البحث في قائمة اللغات
            sourceLangSearch.addEventListener('input', (e) => {
                populateLanguageList(sourceLangList, e.target.value, (code, name) => {
                    sourceLangValue.value = code;
                    sourceLangName.textContent = name;
                    sourceLangDropdown.classList.add('hidden');
                });
            });

            targetLangSearch.addEventListener('input', (e) => {
                populateLanguageList(targetLangList, e.target.value, (code, name) => {
                    targetLangValue.value = code;
                    targetLangName.textContent = name;
                    targetLangDropdown.classList.add('hidden');
                });
            });
            
            // إخفاء القوائم المنسدلة عند النقر خارجها
            document.addEventListener('click', (e) => {
                if (!sourceLangBtn.contains(e.target) && !sourceLangDropdown.contains(e.target)) {
                    sourceLangDropdown.classList.add('hidden');
                }
                if (!targetLangBtn.contains(e.target) && !targetLangDropdown.contains(e.target)) {
                    targetLangDropdown.classList.add('hidden');
                }
            });

            // زر تبديل اللغات
            swapBtn.addEventListener('click', () => {
                const sourceCode = sourceLangValue.value;
                const sourceName = sourceLangName.textContent;
                const targetCode = targetLangValue.value;
                const targetName = targetLangName.textContent;

                sourceLangValue.value = targetCode;
                sourceLangName.textContent = targetName;
                targetLangValue.value = sourceCode;
                targetLangName.textContent = sourceName;

                // تبديل محتوى مربعات النص أيضًا
                const sourceContent = sourceText.value;
                const targetContent = targetText.value;
                sourceText.value = targetContent;
                targetText.value = sourceContent;
            });
            
            // Function to convert base64 to ArrayBuffer
            function base64ToArrayBuffer(base64) {
                const binaryString = atob(base64);
                const len = binaryString.length;
                const bytes = new Uint8Array(len);
                for (let i = 0; i < len; i++) {
                    bytes[i] = binaryString.charCodeAt(i);
                }
                return bytes.buffer;
            }

            // Function to convert PCM audio data to a WAV file Blob
            function pcmToWav(pcmData, sampleRate) {
                const pcm16 = new Int16Array(pcmData);
                const dataLength = pcm16.length * 2;
                const buffer = new ArrayBuffer(44 + dataLength);
                const view = new DataView(buffer);

                // WAV header
                const writeString = (str) => {
                    for (let i = 0; i < str.length; i++) {
                        view.setUint8(i, str.charCodeAt(i));
                    }
                };
                writeString('RIFF');
                view.setUint32(4, 36 + dataLength, true);
                writeString('WAVE');
                writeString('fmt ');
                view.setUint32(16, 16, true);
                view.setUint16(20, 1, true); // Mono channel
                view.setUint16(22, 1, true); // Mono channel
                view.setUint32(24, sampleRate, true);
                view.setUint32(28, sampleRate * 2, true);
                view.setUint16(32, 2, true);
                view.setUint16(34, 16, true); // 16 bits per sample
                writeString('data');
                view.setUint32(40, dataLength, true);

                // Write PCM data
                let offset = 44;
                for (let i = 0; i < pcm16.length; i++, offset += 2) {
                    view.setInt16(offset, pcm16[i], true);
                }
                
                return new Blob([view], { type: 'audio/wav' });
            }

            // دالة لتحويل النص إلى صوت
            async function speakText() {
                const textToSpeak = targetText.value.trim();
                const targetLang = targetLangValue.value;

                if (!textToSpeak) {
                    showMessage('لا يوجد نص للاستماع إليه.', 'info');
                    return;
                }

                // Show loading indicator
                loadingIndicator.classList.remove('hidden');

                try {
                    const apiKey = "";
                    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`;

                    const payload = {
                        contents: [{ parts: [{ text: textToSpeak }] }],
                        generationConfig: {
                            responseModalities: ["AUDIO"],
                            speechConfig: {
                                voiceConfig: {
                                    prebuiltVoiceConfig: { voiceName: "Artemis" }
                                }
                            }
                        },
                        model: "gemini-2.5-flash-preview-tts"
                    };

                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });

                    if (!response.ok) {
                        const errorText = await response.text();
                        console.error('TTS API call failed:', response.status, errorText);
                        throw new Error(`خطأ في تحويل النص إلى صوت: ${response.status}.`);
                    }

                    const result = await response.json();
                    const part = result?.candidates?.[0]?.content?.parts?.[0];
                    const audioData = part?.inlineData?.data;
                    const mimeType = part?.inlineData?.mimeType;

                    if (audioData && mimeType && mimeType.startsWith("audio/")) {
                        const sampleRateMatch = mimeType.match(/rate=(\d+)/);
                        const sampleRate = sampleRateMatch ? parseInt(sampleRateMatch[1], 10) : 16000;
                        
                        const pcmData = base64ToArrayBuffer(audioData);
                        const wavBlob = pcmToWav(pcmData, sampleRate);
                        const audioUrl = URL.createObjectURL(wavBlob);
                        
                        const audio = new Audio(audioUrl);
                        audio.play();
                        
                        showMessage('جارٍ تشغيل الصوت...', 'success');
                    } else {
                        throw new Error('لم يتم الحصول على بيانات صوتية صالحة.');
                    }
                } catch (error) {
                    console.error('TTS failed:', error);
                    showMessage(`حدث خطأ أثناء تشغيل الصوت: ${error.message}`, 'error');
                } finally {
                    loadingIndicator.classList.add('hidden');
                }
            }

            // إضافة مستمعي الأحداث لأيقونات الميكروفون ومكبر الصوت
            sourceMicIcon.addEventListener('click', () => {
                const langCode = sourceLangValue.value;
                startSpeechRecognition(sourceText, langCode, sourceMicIcon);
            });
            targetSpeakerIcon.addEventListener('click', speakText);

            // دالة ترجمة النص
            async function translateText() {
                const textToTranslate = sourceText.value.trim();
                const sourceLang = sourceLangValue.value;
                const targetLang = targetLangValue.value;

                if (!textToTranslate) {
                    showMessage('الرجاء إدخال نص للترجمة.', 'error');
                    return;
                }
                
                if (sourceLang === targetLang) {
                    showMessage('لغة المصدر والهدف لا يمكن أن تكونا متطابقتين.', 'error');
                    return;
                }

                loadingIndicator.classList.remove('hidden');
                translateBtn.disabled = true;

                try {
                    const apiKey = "";
                    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                    const prompt = `Translate the following text from ${sourceLang} to ${targetLang}: ${textToTranslate}. Only return the translated text without any extra commentary.`;

                    const payload = {
                        contents: [{ parts: [{ text: prompt }] }],
                    };

                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });

                    if (!response.ok) {
                        const errorText = await response.text();
                        console.error('API call failed:', response.status, errorText);
                        throw new Error(`حدث خطأ في واجهة برمجة التطبيقات: ${response.status}. يرجى المحاولة مرة أخرى لاحقًا.`);
                    }

                    const result = await response.json();
                    const translatedText = result?.candidates?.[0]?.content?.parts?.[0]?.text;

                    if (translatedText) {
                        targetText.value = translatedText;
                        targetSpeakerIcon.classList.remove('hidden');
                        showMessage('تمت الترجمة بنجاح!', 'success');
                    } else {
                        throw new Error('لم يتم الحصول على ترجمة. حاول مرة أخرى.');
                    }
                } catch (error) {
                    console.error('Translation failed:', error);
                    showMessage(`حدث خطأ أثناء الترجمة: ${error.message}`, 'error');
                } finally {
                    loadingIndicator.classList.add('hidden');
                    translateBtn.disabled = false;
                }
            }

            // الاستماع لزر الترجمة
            translateBtn.addEventListener('click', translateText);
            
            // إضافة خاصية الترجمة عند الضغط على Enter في مربع النص المصدر
            sourceText.addEventListener('keydown', (e) => {
                if (e.key === 'Enter' && !e.shiftKey) {
                    e.preventDefault(); // منع الإدخال من إضافة سطر جديد
                    translateText();
                }
            });
        });
    </script>
</body>
</html>
