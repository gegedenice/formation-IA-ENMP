# Modèles audio : Speech-to-Text (STT) et Text-to-Speech (TTS)

Les modèles audio contemporains (STT et TTS) reposent sur les mêmes briques conceptuelles que les modèles de langage modernes : transformers, embeddings multimodaux, encoders profonds, ou parfois modèle de diffusion.

Cependant la plupart du temps ce sont des modèles très légers po

## Speech-to-Text (STT) — Reconnaissance vocale

Les modèles STT modernes fonctionnent en deux grandes étapes :

* Encodage spectral
  * L’audio est transformé en spectrogramme , qui équivaut à une image 2D traitée comme une séquence de patches.
* Encoder transformer
  * L’encoder extrait une représentation latente très dense du signal.
  * Exemples : OpenAI Whisper, Facebook Wav2Vec2, SpeechT5-Encoder, Distil-Whisper.
* Décodage textuel
  * S'appuie soit sur un décodeur transformer (Whisper), soit une tête linéaire si le modèle prédit directement les tokens (Wav2Vec2 → CTC).

| Famille              | Exemple  | Particularité                                                             |
| -------------------- | -------- | ------------------------------------------------------------------------- |
| **CTC-based models** | Wav2Vec2 | Rapides, efficaces, moins bons sur la ponctuation & les langues multiples |
| **Encoder–Decoder**  | Whisper  | Robuste, multilingue, gère les accents, les bruits, les horodatages       |

Exemple : [https://kyutai.org/next/stt](https://kyutai.org/next/stt)

## TTS

La synthèse vocale moderne se fait en deux étapes :

* Acoustic Model (texte → spectrogramme)
  * Généralement un transformer (FastSpeech2, SpeechT5, VITS sans alignement explicite) apprend la prosodie, le rythme, l’intonation.
* Vocoder (spectrogramme → onde sonore)
  * Le décodage basés sur des modèles de diffusion (DiffWave) ou GAN (HiFi-GAN) génèrent un audio expressif.

#### 📌 Trois types d’approche

| Type                | Exemple     | Notes                                     |
| ------------------- | ----------- | ----------------------------------------- |
| **FastSpeech-like** | FastSpeech2 | Ultra rapide, qualité correcte            |
| **End-to-end**      | VITS        | Très naturels, entraînement plus complexe |
| **Diffusion TTS**   | Grad-TTS    | Qualité supérieure, plus lent             |

**Exemples**

[https://kyutai.org/next/tts](https://kyutai.org/next/tts)

[https://huggingface.co/spaces/neuphonic/neutts-air](https://huggingface.co/spaces/neuphonic/neutts-air)

Podcast à partir de conversation : [https://huggingface.co/spaces/yasserrmd/VibeVoice](https://huggingface.co/spaces/yasserrmd/VibeVoice)

## Complément : les modèles ASR _temps réel_ (Real-Time / Streaming ASR)

Les modèles ASR très rapides (Automatic Speech Recognition) sont conçus pour fournir une transcription _pendant que la personne parle_, avec une latence de quelques dizaines de millisecondes.\
Ils diffèrent des modèles STT “offline” comme Whisper par l’architecture et le mode de traitement.

Ils sont basés sur du streaming incrémental où l’audio n’est pas traité en un seul bloc mais en chunks (ex. 20–40 ms) dont le modèle met à jour la transcription au fur et à mesure.
