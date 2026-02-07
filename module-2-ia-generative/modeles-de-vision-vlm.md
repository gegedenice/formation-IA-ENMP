# Modèles de vision VLM

## Modèles de vision

Historiquement les réseaux de neurones convolutionnels CNNs étaient privilégiés pour le deep learning appliqué aux tâches de computer vision, mais à l’instar des modèles de NLP pour les corpus textuels, les modèles d’IA dédiés à l’analyse d’images et pré-entraînés sur de grands corpus ont aussi amorcé une transition vers des architectures inspirées des Transformers, notamment avec l’émergence du Vision Transformer [ViT](https://huggingface.co/docs/transformers/model_doc/vit) (qui convertit des portions d'images en séquences de tokens) et ses nombreuses variantes (Swin Transformer, DeiT, etc...)

**Tâches**

* classification d’image
* segmentation
* détection ou localisation d’objects

## Vision Language models (VLMs ) : combinaision modèles de vision et modèles de langage

Un VLM combine un LLM pré-entraîné avec un encodeur d’image associé. Le vision encoder génère des embeddings d’image qui sont ensuite projetés dans l’espace des embeddings de texte par un petit module complémentaire - un projecteur- , afin d’aligner les deux représentations vectorielles dans un espace commun.

Les vision encoders modèles qui sont capables de produire des embeddings visuels sont eux-mêmes pré-entraînés sur des paires image-texte pour capturer les correspondances sémantiques entre texte et image. Parmi les plus utilisés, on trouve notamment [CLIP](https://huggingface.co/openai/clip-vit-base-patch32) d'OpenAI, [BLIP](https://huggingface.co/Salesforce/blip-image-captioning-base), ou [DINOv2](https://huggingface.co/facebook/dinov2-base).

**Tâches classiques des modèles de vision (segmentation, détection d'objects, OCR, détection de layout...)**

Exemple avec Florence2 : [https://huggingface.co/spaces/gokaygokay/Florence-2](https://huggingface.co/spaces/gokaygokay/Florence-2)

**Interaction conversationnelle avec des contenus multimédias (image-to-text)**

Exemple avec famille des modèles de vision Qwen : [https://huggingface.co/spaces/prithivMLmods/Qwen3-VL-Outpost](https://huggingface.co/spaces/prithivMLmods/Qwen3-VL-Outpost)

**Génération d’images à partir d’instructions (text-to-image)**

Exemple avec modèle black-forest-labs/FLUX.1-dev : [https://huggingface.co/spaces/black-forest-labs/FLUX.1-dev](https://huggingface.co/spaces/black-forest-labs/FLUX.1-dev)

**Modèles de vision avec raisonnement**

* Modèle zai-org/GLM-4.1V-9B-Thinking : [https://huggingface.co/spaces/prithivMLmods/Multimodal-VLM-v1.0](https://huggingface.co/spaces/prithivMLmods/Multimodal-VLM-v1.0)
* Modèle moonshotai/Kimi-VL-A3B-Thinking-2506 : [https://huggingface.co/spaces/moonshotai/Kimi-VL-A3B-Thinking](https://huggingface.co/spaces/moonshotai/Kimi-VL-A3B-Thinking)

**Transcription OCR et HTR**

Comparatifs de modèles (Nanonets OCR, SmolDocLing, dots.OCR, Rolmo.OCR...) :

* [https://huggingface.co/spaces/prithivMLmods/Multimodal-OCR2](https://huggingface.co/spaces/prithivMLmods/Multimodal-OCR2)
* [https://huggingface.co/spaces/prithivMLmods/Multimodal-OCR3](https://huggingface.co/spaces/prithivMLmods/Multimodal-OCR3)
* [https://huggingface.co/spaces/davanstrien/ocr-time-machine](https://huggingface.co/spaces/davanstrien/ocr-time-machine)

**Extraction de données structurées avec raisonnement**

Exemple avec modèle NuMarkdown-8B-Thinking : [https://huggingface.co/spaces/numind/NuMarkdown-8B-Thinking](https://huggingface.co/spaces/numind/NuMarkdown-8B-Thinking)

**Transformation d'images (rotation...)**

Exemple avec modèles Qwen Images Edit : [https://huggingface.co/spaces/black-forest-labs/FLUX.1-dev](https://huggingface.co/spaces/black-forest-labs/FLUX.1-dev)

**Détection d'objets en flux continu dans vidéo**

Exemple avec modèles SAM de Meta : [https://huggingface.co/spaces/merve/SAM3-video-segmentation](https://huggingface.co/spaces/merve/SAM3-video-segmentation)



