<h1 align="center">
 babyagi
</h1>

# Readme translations:

<kbd>[<img title="Portuguese" alt="Portuguese" src="https://cdn.staticaly.com/gh/hjnilsson/country-flags/master/svg/br.svg" width="22">](docs/README-pt-br.md)</kbd>
<kbd>[<img title="Russian" alt="Russian" src="https://cdn.staticaly.com/gh/hjnilsson/country-flags/master/svg/ru.svg" width="22">](docs/README-ru.md)</kbd>
<kbd>[<img title="Slovenian" alt="Slovenian" src="https://cdn.staticaly.com/gh/hjnilsson/country-flags/master/svg/si.svg" width="22">](docs/README-si.md)</kbd>
<kbd>[<img title="Spanish" alt="Spanish" src="https://cdn.staticaly.com/gh/hjnilsson/country-flags/master/svg/es.svg" width="22">](docs/README-es.md)</kbd>
<kbd>[<img title="Turkish" alt="Turkish" src="https://cdn.staticaly.com/gh/hjnilsson/country-flags/master/svg/tr.svg" width="22">](docs/README-tr.md)</kbd>
<kbd>[<img title="Ukrainian" alt="Ukrainian" src="https://cdn.staticaly.com/gh/hjnilsson/country-flags/master/svg/ua.svg" width="22">](docs/README-ua.md)</kbd>
<kbd>[<img title="简体中文" alt="Simplified Chinese" src="https://cdn.staticaly.com/gh/hjnilsson/country-flags/master/svg/cn.svg" width="22">](docs/README-cn.md)</kbd>
<kbd>[<img title="繁體中文 (Traditional Chinese)" alt="繁體中文 (Traditional Chinese)" src="https://cdn.staticaly.com/gh/hjnilsson/country-flags/master/svg/tw.svg" width="22">](docs/README-zh-tw.md)</kbd>
<kbd>[<img title="Hindi" alt="Hindi" src="https://cdn.staticaly.com/gh/hjnilsson/country-flags/master/svg/in.svg" width="22">](docs/README-hi.md)</kbd>

# लक्ष्य

यह पायथन स्क्रिप्ट एक ऐपीआई-सशक्त कार्य प्रबंधन सिस्टम का एक उदाहरण है। इस सिस्टम में OpenAI और Pinecone ऐपीआई का उपयोग किया जाता है ताकि कार्य बनाएं, प्राथमिकता दें और निष्पादित करें। इस सिस्टम की मुख्य विचारधारा यह है कि यह पिछले कार्यों के परिणाम और पूर्वनिर्धारित उद्देश्य पर आधारित कर कार्य बनाता है। स्क्रिप्ट फिर OpenAI की प्राकृतिक भाषा प्रसंस्करण (NLP) क्षमताओं का उपयोग करके उद्देश्य पर आधारित नए कार्य बनाता है, और Pinecone का उपयोग करके कार्य परिणाम संदर्भ के लिए संग्रहीत करता है। यह मूल कार्य-निर्देशित स्वचालित एजेंट (28 मार्च, 2023) का संक्षेप में संस्कर्ण है।

यह README निम्नलिखित विषयों पर परिचर्चा करेगा:

स्क्रिप्ट कैसे काम करता है

स्क्रिप्ट का उपयोग कैसे करें

समर्थित मॉडल

स्क्रिप्ट को निरंतर चलाने के बारे में चेतावनी

स्क्रिप्ट कैसे काम करता है<a name="how-it-works"></a>

यह स्क्रिप्ट एक अनंत लूप चलाकर काम करता है जो निम्नलिखित चरणों को पूरा करता है:

1. कार्य सूची से पहला कार्य निकालता है।
2. कार्य को क्रियान्वयन एजेंट को भेजता है, जो संदर्भ पर आधारित कार्य को पूरा करने के लिए OpenAI की API का उपयोग करता है।
3. परिणाम को समृद्ध करता है और इसे Pinecone में संग्रहीत करता है।
4. नए कार्य बनाता है और पिछले कार्य के परिणाम और उद्देश्य पर आधारित कार्य सूची की पुनर्योजना करता है।
</br>

एक्षेक्यूशन एजेंट() फ़ंक्शन में ओपनएआई एपीआई का उपयोग किया जाता है। यह दो पैरामीटर लेता है: उद्देश्य और कार्य। फिर यह ओपनएआई की एपीआई को एक प्रॉम्प्ट भेजता है, जो कार्य के परिणाम को लौटाता है। प्रॉम्प्ट में एक एआई सिस्टम के कार्य का विवरण, उद्देश्य और कार्य शामिल होता है। परिणाम फिर स्ट्रिंग के रूप में लौटाया जाता है।
</br>
टास्क_क्रिएशन_एजेंट() फ़ंक्शन में ओपनएआई की एपीआई का उपयोग उद्देश्य और पिछले कार्य के परिणाम पर आधारित नए कार्य बनाने के लिए किया जाता है। यह फ़ंक्शन चार पैरामीटर लेता है: उद्देश्य, पिछले कार्य का परिणाम, कार्य विवरण, और वर्तमान कार्य सूची। फिर यह ओपनएआई की एपीआई को एक प्रॉम्प्ट भेजता है, जो नई कार्यों की सूची को स्ट्रिंग की रूप में लौटाता है। फ़ंक्शन फिर एक डिक्शनरी की सूची के रूप में नए कार्यों को लौटाता है, हर एक डिक्शनरी में कार्य का नाम शामिल होता है।
</br>
प्राथमिकता_एजेंट() फ़ंक्शन में ओपनएआई की एपीआई का उपयोग किया जाता है ताकि कार्य सूची को पुनर्विचारित किया जा सके। फ़ंक्शन एक पैरामीटर लेता है, वर्तमान कार्य का आईडी। फिर यह ओपनएआई की एपीआई को एक प्रॉम्प्ट भेजता है, जो नई कार्य सूची को नंबरदार सूची के रूप में लौटाता है।

अंत में, स्क्रिप्ट पाइनकोन का उपयोग करके कार्य परिणामों को संग्रहीत करता है और प्राप्त करता है। स्क्रिप्ट एक पाइनकोन इंडेक्स बनाता है जो YOUR_TABLE_NAME चयनित टेबल नाम पर आधारित है। पाइनकोन का उपयोग करके कार्य परिणामों को इंडेक्स में संग्रहीत करता है, साथ ही कार्य का नाम और किसी अतिरिक्त मेटाडाटा को भी संग्रहीत करता है।







# कैसे उपयोग करें<a name="how-to-use"></a>

स्क्रिप्ट का उपयोग करने के लिए, आपको इन चरणों का पालन करना होगा:

1. `git clone https://github.com/yoheinakajima/babyagi.git` के माध्यम से रिपॉज़िटरी को क्लोन करें और क्लोन किए गए रिपॉज़िटरी में cd करें।
2. आवश्यक पैकेज इंस्टॉल करें:  `pip install -r requirements.txt`
3. .env.example फ़ाइल की कॉपी को .env में कॉपी करें: cp .env.example .env। यहां आपको निम्नलिखित चर निर्धारित करने होंगे।
4. अपने OpenAI और Pinecone API कुंजियों को OPENAI_API_KEY, OPENAPI_API_MODEL, और PINECONE_API_KEY चर में सेट करें।
5. Pinecone पर्यावरण को PINECONE_ENVIRONMENT चर में सेट करें।
6. कौन से टेबल में कार्य परिणाम संग्रहीत किए जाएंगे के नाम को TABLE_NAME चर में सेट करें।
7. (वैकल्पिक) कार्य प्रबंधन सिस्टम का उद्देश्य OBJECTIVE चर में सेट करें।
8. (वैकल्पिक) सिस्टम का पहला कार्य INITIAL_TASK चर में सेट करें।
9. स्क्रिप्ट चलाएं।

सभी वैकल्पिक मान भी कमांड लाइन पर निर्दिष्ट किए जा सकते हैं।

# एक डॉकर कंटेनर के अंदर चलाना

पूर्वापेक्षा के रूप में, आपको डॉकर और डॉकर-कम्पोज़ स्थापित करने की आवश्यकता होगी। डॉकर डेस्कटॉप सबसे सरल विकल्प है https://www.docker.com/products/docker-desktop/

सिस्टम को डॉकर कंटेनर के अंदर चलाने के लिए, अपनी .env फ़ाइल को उपरोक्त चरणों के अनुसार सेटअप करें और फिर निम्नलिखित को चलाएं:

```
docker-compose up
```

# समर्थित मॉडल<a name="supported-models"></a>

यह स्क्रिप्ट सभी OpenAI मॉडल्स के साथ काम करता है, साथ ही Llama के माध्यम से भी, Llama.cpp के जरिए। डिफ़ॉल्ट मॉडल है gpt-3.5-turbo। अलग मॉडल का उपयोग करने के लिए, OPENAI_API_MODEL के माध्यम से या कमांड लाइन का उपयोग करके उसे निर्दिष्ट करें।

## ल्लामा (Llama)

Llama.cpp ((https://github.com/ggerganov/llama.cpp) के नवीनतम संस्करण को डाउनलोड करें और निर्देशों का पालन करें। आपको ल्लामा मॉडल वेट्स भी चाहिए होंगे।

- **किसी भी परिस्थिति में इस रिपॉजिटरी में IPFS, magnet लिंक या किसी भी अन्य मॉडल डाउनलोड की जाने वाली लिंक को साझा न करें, समस्त स्थानों पर, समस्याओं, चर्चाओं या पुल अनुरोधों में समेत। वे तुरंत हटा दिए जाएंगे।**

इसके बाद लिंक `llama/main` को llama.cpp/main और `models` को उस फ़ोल्डर से जो आपके पास Llama मॉडल वेट्स हैं, से लिंक करें। फिर स्क्रिप्ट को `OPENAI_API_MODEL=llama` or `-l` आर्गुमेंट के साथ चलाएं।

# चेतावनी<a name="continous-script-warning"></a>

यह स्क्रिप्ट एक कार्य प्रबंधन प्रणाली का हिस्सा के रूप में निरंतर चलाने के लिए डिज़ाइन किया गया है। इस स्क्रिप्ट को निरंतर चलाने से उच्च API उपयोग हो सकता है, इसलिए कृपया इसका जिम्मेदार उपयोग करें। साथ ही, स्क्रिप्ट को ठीक से सेट अप करने के लिए ओपनएआई और पाइनकोन एपीआई की आवश्यकता होती है, इसलिए स्क्रिप्ट को चलाने से पहले आपने एपीआई को सेट अप कर लें।

# योगदान

बिना कहे ही, BabyAGI अपने शिशु अवस्था में है और इसलिए हम अभी भी इसकी दिशा निर्धारित कर रहे हैं और वहां पहुँचने के लिए कदम। वर्तमान में BabyAGI का एक मुख्य डिज़ाइन लक्ष्य सीधा है ताकि यह समझने में आसान हो और इस पर निर्माण करने में सुविधाजनक हो। इस सरलता को बनाए रखने के लिए हम सभी से विनम्रता से अनुरोध करते हैं कि आप निम्नलिखित दिशा-निर्देशों का पालन करें जब PRs (पुल रिक्वेस्ट्स) को सबमिट करते हैं:

- बड़े पैमाने पर सुधार के बजाय छोटे, मॉड्यूलर परिवर्तनों पर ध्यान केंद्रित करें।
- नई सुविधाओं को प्रस्तुत करते समय विशिष्ट उपयोग मामले का विस्तृत विवरण प्रदान करें।

एक नोट @yoheinakajima (5 अप्रैल, 2023):

> मुझे पता है कि पीआर (पुल रिक्वेस्ट) की बढ़ती हुई संख्या है, आपकी सब्र की कीमत करता हूँ - क्योंकि मैं गिटहब/ओपन सोर्स में नया हूँ और इस हफ्ते मेरी समय की उपलब्धता की योजना नहीं बनाई गई थी। पुनर्निर्देशन के संबंध में, मेरे दिल में सम्मिलित विचार हैं कि एक मूल बेबी एजीआई को सीधा रखने और इसे एक मंच के रूप में उपयोग करने की ओर झुक रहा हूँ जो इसे विस्तृत करने की विभिन्न पहुंचों का समर्थन करेगा और प्रोत्साहित करेगा (जैसे कि एक दिशा में BabyAGIxLangchain)। मेरी मान्यता है कि विभिन्न एकांत मत वाले दृष्टिकोण में जाने योग्य दृष्टिकोण हैं, और मौजूदा में तुलना और चर्चा करने के लिए एक केंद्रीय स्थान की मूल्य है। जल्द ही अधिक अपडेट आ रहे हैं।
मैं गिटहब और ओपन सोर्स में नया हूँ, इसलिए कृपया स्थिरता बनाए रखें क्योंकि मैं इस परियोजना का ठीक से प्रबंधन सीख रहा हूँ। मेरे द्वारा एक वीसी फर्म चलते हैं, इसलिए रोज रात को अपने बच्चों को सुलाने के बाद पीआर और मुद्दों की जांच करूंगा - जो हर रात नहीं हो सकता। समर्थन लाने के विचार पर खुले हैं, जल्द ही इस खंड को अपडेट करूंगा (अपेक्षाएं, दृष्टिकोण, आदि)। बहुत से लोगों से बातचीत कर रहा हूँ और सीख रहा हूँ - अपडेट की प्रतीक्षा करें!

# पूर्व कथा

बेबी एजीआई (BabyAGI) एक संक्षेप में आदर्श संस्करण है जो कार्य-चलित स्वचालित एजेंट (28 मार्च, 2023) का है जो ट्विटर पर साझा किया गया था। यह संस्करण 140 पंक्तियों में गिर गया है: 13 टिप्पणियाँ, 22 खाली पंक्तियाँ, और 105 कोड। रेपो का नाम मूल स्वचालित एजेंट की प्रतिक्रिया में आया - लेखक का अर्थ यह नहीं है कि यह एजीआई है।

@yoheinakajima द्वारा प्यार के साथ बनाया गया है, जो एक शौकिया कैपिटलिस्ट (जानने के लिए देखना चाहूंगा कि आप क्या बना रहे हैं!)।