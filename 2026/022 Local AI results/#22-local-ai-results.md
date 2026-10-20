# Lokale AI resultaten

![Cylon AI](images/cylon.jpg "Cylon AI Image")

## Inleiding

Ik ben nu wat meer bezig met het gebruik van **AI**, zowel algemene dingen als voor het uitleggen van **code**. Ik laat de **AI** zelf geen **code** schrijven, ik gebruik het als hulpmiddel.

Voor de algemene vragen gebruik ik **Gemini** en **Claude**, maar als ik iets anders moet weten over specifieke **code** of werkwijzen dan gebruik ik lokale **AI**.

Als **prompt** gebruik ik een simpele **prompt** namelijk

	How do I implement a linked list in C

## LLM setups

In deze tests gebruik ik 3 opstellingen met lokale **LLMs**

1. MacBook Pro M1 Pro 16GB (bandbreedte: 200GB/s)
2. Mac Pro 2019 W-3235 Xeon 32GB (bandbreedte: 140GB/s)
3. Mac Pro 2019 W-3235 Xeon 32GB 2x Radeon 580X (bandbreedte: 218GB/s)

De **MacBook** gebruik ik als basis om te kijken wat de andere systemen doen. De **Mac Pro** draait zonder **GPU** op **Llama.cpp** en met 2x **Radeon 580X** op **ToshLLM**.

Als client gebruik ik **AIChat CLI** van **Sigoden**.

Om de LLMs te starten gebruik ik volgende parameters voor de  **Llama.cpp** commando

	llama-server -m /Users/admin/llamacpp/llama.cpp/models/gemma-4-12B-it-Q4_K_M.gguf -ngl 0 -t 10 --host 0.0.0.0 --port 8888 --perf -c 32768 -ctk q4_0 -ctv q4_0 -fa on
	
En voor **ToshLLM** (enkel als ik de 2 **GPUs** wil gebruiken)

	GGML_METAL_VRAM_RESERVE_MB=1024 GGML_METAL_DEVICE_LIST=0,1 TOSH_FA_AMD=1 TOSH_MGPU_EVENTS=1 /Applications/ToshLLM.app/Contents/Resources/bin/llama-server -m /Users/admin/llamacpp/llama.cpp/models/gemma-4-12B-it-Q4_K_M.gguf -ngl 48 -sm layer -ts 1,1 --host 0.0.0.0 --port 8888 --perf -c 32768 -ctk q4_0 -ctv q4_0 -fa on
	
## Modellen

Ik gebruik 4 **modellen** voor deze tests, hoewel ik dagelijks enkel **gemma4** gebruik als de **LLM** moet denken en **Qwen2.5-Coder** voor alle andere **programmeer** vragen.

De modellen zijn

	1. DeepSeek-R1-Distill-Qwen-14B-Q4_K_M.gguf		8,99GB
	2. Qwen2.5-Coder-14B-Q4_K_M.gguf				8,99GB
	3. gemma-1.1-7b-it-Q6_K.gguf					7,01GB
	4. gemma-4-12B-it-Q4_K_M.gguf					7,14GB

## Prompt Processing

![Prompt Processing Tokens/seconde](images/1-PPtks.png)

Voor **prompt processing** zie je bij het **niet-denkend** model dat **setup 3** wat trager is. Bij vergelijking met **setup 1** (de basis) zie je dat zelfs de snelste **setup 3**  minder tokens per seconde verwerkt. Hier is dan ook enkel een simpele prompt gebruikt (geen bestanden inladen).

Dit zie je ook in de milliseconden voor **prompt processing**. **setup3** is trager dan de basis **setup 1**

![Prompt Processing Time in Milliseconds](images/2-PPms.png)

Al bij al niet zo een groot verschil, maar wel opmerkelijk. Je zou eigenlijk verwachten dat **setup 3** de snelste is.

## Tokens per seconde

Bij de **tokens per seconde** zie je wel dat **setup 3** meer **tokens** verwerkt per seconde.

![Tokens per Seconde](images/3-tks.png)

Niet zo een drastisch verschil tussen **setup 1** en **setup 3**, bij **model 4** is **setup 3** zelfs een beetje trager. Nu was dit een verwacht resultaat want de **geheugenbandbreedte** verschilt ook niet zoveel (200GB/s voor **setup 1** en 218GB/s voor **setup 3**).

Maar wat voor mij belangrijker is het verschil tussen **setup 2** en **setup 3** namelijk zonder of met **GPUs**. Daar merk je het ook dat **setup 3** sneller is, de woorden verschijnen sneller dan men kan lezen op **setup 3**. Terwijl bij **setup 2** je goed kon meelezen.

Nu als je niet verwacht dat je de snelheid van de **frontier modellen** moet hebben dan is zelfs **7 tokens perseconde** genoeg om niet onbruikbaar te zijn, je moet gewoon wat geduld hebben bij het denkend **model**.

![Total Time in Seconds](images/4-ts.png)

Qua totale tijd van **prompt processing** tot het einde van de output is **setup 1** iets sneller dan **setup 3**, maar weer is het verschil tussen **setup 2** en **setup 3** belangrijker voor mij (hoewel dat ook niet veel verschilt).

## Aantal tokens gemaakt

![Total number of tokens](images/5-nt.png)

Daar zie je bij de 2 belangrijkste **modellen** dat het vrij gelijk loopt tussen de **setups**. Dat beïnvloedt dus ook het aantal **tokens per seconde** en **totale tijd**.

## Toekomst

Nu zou ik mettertijd graag 2x **Radeon W5700X 16GB** **GPUs** aanschaffen. Hier verwacht ik dan wel een duidelijker sneller resultaat omdat de **geheugenbandbreedte** hoger is, namelijk **500GB/s**. In theorie zou dit een verdubbeling in de resulaten betekenen, dus rond de 20 **tokens/seconde** en de helft van de totale tijd per **LLM**. Die 2de **GPU** is er enkel om dezelfde **modellen** te gebruiken maar wel meer ruimte voor **context**. Nu gebruik ik **32k context**, maar dan zou ik misschien tot **128k context** kunnen gaan.

## Conclusie

Ik ben tevreden met mijn aankoop van de **Mac Pro 2019 Rack** voor het gebruik van **lokale AI**, nu gebruik ik hem ook voor andere dingen, maar hier ging het om **lokale AI** tests. Als je echt het snelst wilt dan moet je kijken naar de **Mac Studio M5 ultra of Max**, als je binnen het **mac** ecosysteem wilt blijven. Maar omdat ik graag wat **tinker** met mijn **computers** en ook gewoon nieuwsgierig ben heb ik de **Mac Pro 2019 Rack** gekocht.