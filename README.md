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
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">
    <div class="w-full max-w-4xl p-8 bg-gray-800 rounded-2xl shadow-lg border border-gray-700">
        <h1 class="text-3xl sm:text-4xl font-bold text-center mb-6 text-indigo-400">مترجم اللغات</h1>
        <p class="text-center text-gray-400 mb-8">قم بالترجمة بين اللغات بسرعة ودقة.</p>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="flex flex-col">
                <div class="flex items-center justify-between mb-2">
                    <label for="source-lang" class="text-sm font-medium text-gray-300">لغة المصدر</label>
                    <select id="source-lang" class="p-2 text-sm bg-gray-700 border border-gray-600 rounded-md text-gray-300 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    </select>
                </div>
                <textarea id="source-text" rows="10" class="w-full p-4 bg-gray-900 border border-gray-700 rounded-xl text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none" placeholder="أدخل النص هنا..."></textarea>
            </div>

            <div class="flex flex-col">
                <div class="flex items-center justify-between mb-2">
                    <label for="target-lang" class="text-sm font-medium text-gray-300">لغة الهدف</label>
                    <select id="target-lang" class="p-2 text-sm bg-gray-700 border border-gray-600 rounded-md text-gray-300 focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    </select>
                </div>
                <textarea id="target-text" rows="10" class="w-full p-4 bg-gray-900 border border-gray-700 rounded-xl text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none" placeholder="النص المترجم سيظهر هنا..." readonly></textarea>
            </div>
        </div>

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
            const sourceLangSelect = document.getElementById('source-lang');
            const targetLangSelect = document.getElementById('target-lang');
            const translateBtn = document.getElementById('translate-btn');
            const loadingIndicator = document.getElementById('loading-indicator');

            // قائمة اللغات المدعومة
            const languages = [
                { code: 'af', name: 'الأفريقانية' },
                { code: 'sq', name: 'الألبانية' },
                { code: 'am', name: 'الأمهرية' },
                { code: 'ar', name: 'العربية' },
                { code: 'hy', name: 'الأرمينية' },
                { code: 'az', name: 'الأذرية' },
                { code: 'eu', name: 'الباسكية' },
                { code: 'be', name: 'البيلاروسية' },
                { code: 'bn', name: 'البنغالية' },
                { code: 'bs', name: 'البوسنية' },
                { code: 'bg', name: 'البلغارية' },
                { code: 'ca', name: 'الكتالونية' },
                { code: 'ceb', name: 'السيبيونو' },
                { code: 'zh', name: 'الصينية' },
                { code: 'co', name: 'الكورسيكية' },
                { code: 'hr', name: 'الكرواتية' },
                { code: 'cs', name: 'التشيكية' },
                { code: 'da', name: 'الدنماركية' },
                { code: 'nl', name: 'الهولندية' },
                { code: 'en', name: 'الإنجليزية' },
                { code: 'eo', name: 'الإسبرانتو' },
                { code: 'et', name: 'الإستونية' },
                { code: 'fi', name: 'الفنلندية' },
                { code: 'fr', name: 'الفرنسية' },
                { code: 'fy', name: 'الفريزية' },
                { code: 'gl', name: 'الجاليكية' },
                { code: 'ka', name: 'الجورجية' },
                { code: 'de', name: 'الألمانية' },
                { code: 'el', name: 'اليونانية' },
                { code: 'gu', name: 'الغوجاراتية' },
                { code: 'ht', name: 'الهايتية' },
                { code: 'ha', name: 'الهوسا' },
                { code: 'haw', name: 'الهاوائية' },
                { code: 'he', name: 'العبرية' },
                { code: 'hi', name: 'الهندية' },
                { code: 'hmn', name: 'الهمونغ' },
                { code: 'hu', name: 'المجرية' },
                { code: 'is', name: 'الآيسلندية' },
                { code: 'ig', name: 'الإيغبو' },
                { code: 'id', name: 'الإندونيسية' },
                { code: 'ga', name: 'الأيرلندية' },
                { code: 'it', name: 'الإيطالية' },
                { code: 'ja', name: 'اليابانية' },
                { code: 'jv', name: 'الجاوية' },
                { code: 'kn', name: 'الكانادية' },
                { code: 'kk', name: 'الكازاخية' },
                { code: 'km', name: 'الخميرية' },
                { code: 'ko', name: 'الكورية' },
                { code: 'ku', name: 'الكردية' },
                { code: 'ky', name: 'القيرغيزية' },
                { code: 'lo', name: 'اللاوية' },
                { code: 'la', name: 'اللاتينية' },
                { code: 'lv', name: 'اللاتفية' },
                { code: 'lt', name: 'الليتوانية' },
                { code: 'lb', name: 'اللوكسمبورغية' },
                { code: 'mk', name: 'المقدونية' },
                { code: 'mg', name: 'المالاجاشية' },
                { code: 'ms', name: 'الماليزية' },
                { code: 'ml', name: 'المالايالامية' },
                { code: 'mt', name: 'المالطية' },
                { code: 'mi', name: 'الماورية' },
                { code: 'mr', name: 'الماراثية' },
                { code: 'mn', name: 'المنغولية' },
                { code: 'ne', name: 'النيبالية' },
                { code: 'no', name: 'النرويجية' },
                { code: 'ny', name: 'النيانجا' },
                { code: 'ps', name: 'الباشتو' },
                { code: 'fa', name: 'الفارسية' },
                { code: 'pl', name: 'البولندية' },
                { code: 'pt', name: 'البرتغالية' },
                { code: 'pa', name: 'البنجابية' },
                { code: 'ro', name: 'الرومانية' },
                { code: 'ru', name: 'الروسية' },
                { code: 'sm', name: 'الساموية' },
                { code: 'gd', name: 'الاسكتلندية الغيلية' },
                { code: 'sr', name: 'الصربية' },
                { code: 'st', name: 'السوتية' },
                { code: 'sn', name: 'الشونا' },
                { code: 'sd', name: 'السندية' },
                { code: 'si', name: 'السنهالية' },
                { code: 'sk', name: 'السلوفاكية' },
                { code: 'sl', name: 'السلوفينية' },
                { code: 'so', name: 'الصومالية' },
                { code: 'es', name: 'الإسبانية' },
                { code: 'su', name: 'السوندية' },
                { code: 'sw', name: 'السواحيلية' },
                { code: 'sv', name: 'السويدية' },
                { code: 'tl', name: 'التاغالوغية' },
                { code: 'tg', name: 'الطاجيكية' },
                { code: 'ta', name: 'التاميلية' },
                { code: 'te', name: 'التيلوغوية' },
                { code: 'th', name: 'التايلاندية' },
                { code: 'tr', name: 'التركية' },
                { code: 'uk', name: 'الأوكرانية' },
                { code: 'ur', name: 'الأوردية' },
                { code: 'uz', name: 'الأوزبكية' },
                { code: 'vi', name: 'الفيتنامية' },
                { code: 'cy', name: 'الويلزية' },
                { code: 'xh', name: 'الخوسا' },
                { code: 'yo', name: 'اليوروبا' },
                { code: 'zu', name: 'الزولو' },
                { code: 'ab', name: 'الأبخازية' },
                { code: 'ace', name: 'الأتشيهية' },
                { code: 'ady', name: 'الأديغية' },
                { code: 'ak', name: 'الأكانية' },
                { code: 'aln', name: 'الألبانية الجيغية' },
                { code: 'ang', name: 'الإنجليزية القديمة' },
                { code: 'anp', name: 'الأنجيكا' },
                { code: 'arc', name: 'الآرامية' },
                { code: 'arn', name: 'الآراوكو' },
                { code: 'asa', name: 'الأسو' },
                { code: 'ast', name: 'الأسترية' },
                { code: 'atj', name: 'الأتيكاميك' },
                { code: 'av', name: 'الأفارية' },
                { code: 'awa', name: 'الأواضية' },
                { code: 'ba', name: 'الباشكيرية' },
                { code: 'ban', name: 'البالينية' },
                { code: 'be-tarask', name: 'البيلاروسية (تاراسكييفيتسا)' },
                { code: 'bi', name: 'البيسلاما' },
                { code: 'bla', name: 'الساسكا' },
                { code: 'bm', name: 'البامبارا' },
                { code: 'br', name: 'البريتونية' },
                { code: 'byn', name: 'البيلين' },
                { code: 'ch', name: 'التشامورو' },
                { code: 'chr', name: 'الشيروكي' },
                { code: 'co', name: 'الكورسيكية' },
                { code: 'cr', name: 'الكري' },
                { code: 'csb', name: 'الكاشوبية' },
                { code: 'cv', name: 'التشوفاشية' },
                { code: 'cy', name: 'الويلزية' },
                { code: 'diq', name: 'الزازاكية' },
                { code: 'dv', name: 'المالديفية' },
                { code: 'dz', name: 'الزونخا' },
                { code: 'ee', name: 'الإيوي' },
                { code: 'efi', name: 'الإفيك' },
                { code: 'fa-af', name: 'الفارسية (أفغانستان)' },
                { code: 'ff', name: 'الفولا' },
                { code: 'fj', name: 'الفيجية' },
                { code: 'fo', name: 'الفاروية' },
                { code: 'fr-ca', name: 'الفرنسية (كندا)' },
                { code: 'fur', name: 'الفريولية' },
                { code: 'ga', name: 'الأيرلندية' },
                { code: 'gd', name: 'الاسكتلندية الغيلية' },
                { code: 'gsw', name: 'الألمانية السويسرية' },
                { code: 'gv', name: 'المانكس' },
                { code: 'ha', name: 'الهوسا' },
                { code: 'haw', name: 'الهاوائية' },
                { code: 'he', name: 'العبرية' },
                { code: 'hif', name: 'الهندية الفيجية' },
                { code: 'hr', name: 'الكرواتية' },
                { code: 'hsb', name: 'الصربية العليا' },
                { code: 'ht', name: 'الهايتية' },
                { code: 'hu', name: 'المجرية' },
                { code: 'hy', name: 'الأرمينية' },
                { code: 'ia', name: 'الإنترلينغوا' },
                { code: 'id', name: 'الإندونيسية' },
                { code: 'ig', name: 'الإيغبو' },
                { code: 'is', name: 'الآيسلندية' },
                { code: 'ja', name: 'اليابانية' },
                { code: 'jv', name: 'الجاوية' },
                { code: 'ka', name: 'الجورجية' },
                { code: 'kab', name: 'القبائلية' },
                { code: 'kbp', name: 'الكابيا' },
                { code: 'kk', name: 'الكازاخية' },
                { code: 'km', name: 'الخميرية' },
                { code: 'kn', name: 'الكانادية' },
                { code: 'ko', name: 'الكورية' },
                { code: 'kri', name: 'الكريولية السيراليونية' },
                { code: 'ku', name: 'الكردية' },
                { code: 'ky', name: 'القيرغيزية' },
                { code: 'la', name: 'اللاتينية' },
                { code: 'lb', name: 'اللوكسمبورغية' },
                { code: 'lg', name: 'اللوغندا' },
                { code: 'ln', name: 'اللينغالا' },
                { code: 'lo', name: 'اللاوية' },
                { code: 'lt', name: 'الليتوانية' },
                { code: 'lv', name: 'اللاتفية' },
                { code: 'mg', name: 'المالاجاشية' },
                { code: 'mi', name: 'الماورية' },
                { code: 'mk', name: 'المقدونية' },
                { code: 'ml', name: 'المالايالامية' },
                { code: 'mn', name: 'المنغولية' },
                { code: 'mr', name: 'الماراثية' },
                { code: 'ms', name: 'الماليزية' },
                { code: 'mt', name: 'المالطية' },
                { code: 'mzn', name: 'المازندرانية' },
                { code: 'my', name: 'البورمية' },
                { code: 'ne', name: 'النيبالية' },
                { code: 'no', name: 'النرويجية' },
                { code: 'ny', name: 'النيانجا' },
                { code: 'oc', name: 'الأوكسيتانية' },
                { code: 'om', name: 'الأورومو' },
                { code: 'or', name: 'الأوريا' },
                { code: 'os', name: 'الأوسيتية' },
                { code: 'pa', name: 'البنجابية' },
                { code: 'pl', name: 'البولندية' },
                { code: 'ps', name: 'الباشتو' },
                { code: 'pt', name: 'البرتغالية' },
                { code: 'qu', name: 'الكويتشوا' },
                { code: 'rm', name: 'الرومانشية' },
                { code: 'ro', name: 'الرومانية' },
                { code: 'ru', name: 'الروسية' },
                { code: 'rw', name: 'الكينيارواندية' },
                { code: 'sa', name: 'السنسكريتية' },
                { code: 'sc', name: 'السردينية' },
                { code: 'sco', name: 'الاسكتلندية' },
                { code: 'sd', name: 'السندية' },
                { code: 'se', name: 'السامي الشمالية' },
                { code: 'sh', name: 'الصربية الكرواتية' },
                { code: 'si', name: 'السنهالية' },
                { code: 'sk', name: 'السلوفاكية' },
                { code: 'sl', name: 'السلوفينية' },
                { code: 'sm', name: 'الساموية' },
                { code: 'sn', name: 'الشونا' },
                { code: 'so', name: 'الصومالية' },
                { code: 'sq', name: 'الألبانية' },
                { code: 'sr', name: 'الصربية' },
                { code: 'ss', name: 'السواتية' },
                { code: 'st', name: 'السوتية' },
                { code: 'su', name: 'السوندية' },
                { code: 'sv', name: 'السويدية' },
                { code: 'sw', name: 'السواحيلية' },
                { code: 'ta', name: 'التاميلية' },
                { code: 'te', name: 'التيلوغوية' },
                { code: 'tg', name: 'الطاجيكية' },
                { code: 'th', name: 'التايلاندية' },
                { code: 'ti', name: 'التيغرينيا' },
                { code: 'tk', name: 'التركمانية' },
                { code: 'tl', name: 'التاغالوغية' },
                { code: 'to', name: 'التونغية' },
                { code: 'tr', name: 'التركية' },
                { code: 'tt', name: 'التتارية' },
                { code: 'tw', name: 'التاوي' },
                { code: 'ty', name: 'التاهيتية' },
                { code: 'ug', name: 'الأويغورية' },
                { code: 'uk', name: 'الأوكرانية' },
                { code: 'ur', name: 'الأوردية' },
                { code: 'uz', name: 'الأوزبكية' },
                { code: 've', name: 'الفيندا' },
                { code: 'vi', name: 'الفيتنامية' },
                { code: 'wa', name: 'الوالونية' },
                { code: 'wo', name: 'الولوف' },
                { code: 'xh', name: 'الخوسا' },
                { code: 'yi', name: 'اليديشية' },
                { code: 'yo', name: 'اليوروبا' },
                { code: 'za', name: 'الزوانغ' },
                { code: 'zh-yue', name: 'الكانتونية' },
                { code: 'zu', name: 'الزولو' }
            ];

            // تعبئة القوائم المنسدلة باللغات
            languages.forEach(lang => {
                const option1 = document.createElement('option');
                option1.value = lang.code;
                option1.textContent = lang.name;
                sourceLangSelect.appendChild(option1);

                const option2 = document.createElement('option');
                option2.value = lang.code;
                option2.textContent = lang.name;
                targetLangSelect.appendChild(option2);
            });

            // ضبط اللغات الافتراضية
            sourceLangSelect.value = 'ar';
            targetLangSelect.value = 'en';

            // دالة ترجمة النص
            async function translateText() {
                const textToTranslate = sourceText.value.trim();
                const sourceLang = sourceLangSelect.value;
                const targetLang = targetLangSelect.value;

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
                    const apiKey = "AIzaSyBZKqnhIkUB0EYXCxVezI7eTK9Se_B97R4"; // تم إضافة المفتاح هنا
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

                    // التحقق من حالة الاستجابة قبل محاولة قراءة الجسم
                    if (!response.ok) {
                        const errorText = await response.text();
                        console.error('API call failed:', response.status, errorText);
                        throw new Error(`حدث خطأ في واجهة برمجة التطبيقات: ${response.status}. يرجى المحاولة مرة أخرى لاحقًا.`);
                    }

                    const result = await response.json();
                    const translatedText = result?.candidates?.[0]?.content?.parts?.[0]?.text;

                    if (translatedText) {
                        targetText.value = translatedText;
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
