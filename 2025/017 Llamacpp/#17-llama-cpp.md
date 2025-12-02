![Man Coding](images/man-coding.jpg "Man coding")

# Llama.cpp en LLMs instalatie

## begin

Nu ik meer met AI werk wil ik deze ook lokaal zonder internet kunnen bereiken. Daarom had ik eerst het plan opgevat om een nieuwe **server** te kopen en daar dan Llama.cpp op te zetten.

Met de research kwam meer en meer naar voor dat dit perfect gaat op de nieuwe **Apple Mx** chips, en omdat ik een **Macbook Pro M1 pro 16GB** bezit ben ik meer gaan opzoeken. Met die **16GB ram** kan ik perfect een lichtere **LLM** draaien. 

Eén **LLM** is specifiek voor het programmeren en eentje voor de **chatGPT** achtige dingen. Dus de gewone non-coding vragen.

Het kan zijn dat je de **xcode-commandline-tools** eerst moet installeren, bij mij stond **xcode** er al op.

In dit artikel ga ik je dus uitleggen hoe ik de installatie heb gedaan, veel leesplezier.

## instaleren Homebrew en cmake

Als eerste gaan we zorgen dat we Homebrew hebben geinstalleerd op het systeem, dit is nodig voor het cmake commando.

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Voer dit uit in de terminal. Als dit geinstalleerd is moet je nog een paar commando's uitvoeren die de installer aan het einde gaf.

Daarna gaan we cmake installeren met het volgende commando

    $ brew install cmake

Laat dit zijn werk doen en als resultaat is cmake geinstalleerd.

## installatie Llama.cpp van source

Ga naar een folder waar je llama.cpp wilt installeren, ik heb gekozen voor een 'llamacpp' folder in mijn home directory. Nu gaan we Llama.cpp clonen van github

    git clone https://github.com/ggml-org/llama.cpp
    cd llama.cpp

Daarna gaan we llama.cpp compileren met de volgende commando's

    $ cmake -B build
    $ cmake --build build --config Release -j 8

Als dit gedaan is heb je Llama.cpp succesvol geinstalleerd op je Macbook Pro.

## Downloaden en installeren LLMs

De volgende stap is het downloaden en uitvoeren van LLMs. Hier heb ik er twee gekozen namelijk

    bartowski/Meta-Llama-3-8B-Instruct-GGUF:Q4_K_M
    bartowski/Qwen2.5-Coder-14B-GGUF:Q4_K_M

De eerste is een algemene zoals chatGPT en de tweede is speciaal voor het programmeren.
Nu gaan we deze de eerste maal downloaden en starten.

Ga naar de map die je gekozen hebt om llama.cpp in te clonen

    $ ~/llamacpp/llama.cpp/build/bin

Nu ga je het commando uitvoeren dat je gebruiokt om de LLM te downloaden (alleen eerste keer) en te starten. Dit kan wel even duren. Als de download klaar is en de llama.cpp server is geestart kun je deze stoppen door ctrl+c te doen in de terminal.

    ~/llamacpp/llama.cpp/build/bin$ ./llama-run -hf bartowski/Meta-Llama-3-8B-Instruct-GGUF:Q4_K_M
    ~/llamacpp/llama.cpp/build/bin$ ./llama-run -hf bartowski/Qwen2.5-Coder-14B-GGUF:Q4_K_M

Nu zijn beide LLMs gedownload en geinstalleerd, vanaf nu heb je het internet niet meer nodig als je vragen stelt, enkel als de AI iets moet opzoeken op het internet.

Als je de LLM start dan kun je deze bereiken op

    localhost:8080

Deze schermafbeelding is gedaan met bartowski/Meta-Llama-3-8B-instruct LLM en bevat de vertaling van deze blogpost van nederelands naar engels.

![translation of this blogpost on a local LLM](images/screenshot-Meta-llama-3-8B-instruct-LLM.png "Translation of this blogpost on a local LLM")

## einde

De twee **LLMs** draaien zeer goed, ik krijg een 17 a 20 tal tokens per seconde, voor mij is dat voldoende.

Bij de aanschaf van mijn volgende **laptop** ga ik zorgen voor meer **ram geheugen** om de context te kunnen vergroten, niet dat ik al tegen de limiet van 4000 tokens ben gekomen, maar dat zal in de toekomst wel gebeuren.

Voila dit is alles, veel plezier.