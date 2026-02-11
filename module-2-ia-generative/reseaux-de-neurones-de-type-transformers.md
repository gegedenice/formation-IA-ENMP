# Réseaux de neurones de type Transformers

{% hint style="info" %}
* <mark style="color:red;">**Article fondateur de l'architecture Transformer (2017) : "Attention is all you need",**</mark>  [<mark style="color:red;">**https://arxiv.org/abs/1706.03762**</mark>](https://arxiv.org/abs/1706.03762)
* <mark style="color:red;">**Représente une rupture dans le NLP profond pour les tâches de compréension du langage naturel**</mark>
{% endhint %}

{% hint style="info" %}
* Architecture de base de (presque) tous les LLMs
  * Réseaux de neurones multi-couches
  * calculs parallélisés (au lieu des calculs séquentiels des réseaux de neurones récurrents classiques)
  * basés sur le principe de l'Attention
  * Propagation : les tokens traversent les piles Transformers, chaque couche appliquant un mécanisme d'auto-attention pour modéliser les relations internes entre mots.
  * Représentation contextuelle dynamique
* Modèles pré-entrainés : Transformers appliqués à des très grandes quantités de données
{% endhint %}

## Tokenisation & conversion token → id : Texte → tokens → ID

{% hint style="danger" %}
Le texte est processé "tel quel" : on conserve tous les mots et signes de ponctuation, ni stemmatisation ni stopwords, en respectant leur ordre d'apparition
{% endhint %}

**Idée clé.** On segmente le texte (tokeniser), normalise (casse, accents), puis on mappe vers des IDs. Le dictionnaire des tokens uniques avec leurs identifiants forme le vocabulaire du modèle

Contraintes : trouver une manière de pouvoir tokeniser des mots qui ne sont pas dans le vocabulaire de référence

Libariries existantes : WordPiece/BPE

Tuto : [https://huggingface.co/docs/transformers/tokenizer\_summary](https://huggingface.co/docs/transformers/tokenizer_summary)

Démos :&#x20;

* [https://platform.openai.com/tokenizer](https://platform.openai.com/tokenizer)
* [https://platform.openai.com/tokenizer](https://platform.openai.com/tokenizer)

Les choix de représentations (Texte → tokens → IDs → vecteurs) ont un impact majeur sur les calculs effectués durant l'entraînement des modèles.

Le dictionnaire des tokens uniques avec leurs identifiants forme le vocabulaire

<table><thead><tr><th width="187">Text</th><th>Tokens</th><th valign="top">Vocabulaire Token -> Id</th><th valign="top">Vocabulaire ID -> Token</th></tr></thead><tbody><tr><td>Peintre officiel de la marine et fondateur de la société coloniale des artistes français, Louis Jules Dumoulin (1860-1924) a parcouru le monde. Entre 1888, date de son premier voyage au long cours et 1897, il effectue deux missions officielles en Asie qui le conduiront à séjourner à trois reprises au Japon mais aussi dans plusieurs autres pays dont la Chine et surtout l’Indochine française, territoire qui fera de ce patriote un colonialiste convaincu et militant. Artiste prolifique et jouissant d’une influence certaine de son vivant, Dumoulin a livré de nombreux tableaux inspirés de ces voyages et exécutés à partir d’études in situ ou de modèles photographiques puisées dans sa riche collection personnelle. Malgré le fait qu’il soit l’un des premiers peintres français à avoir effectué le voyage jusqu’au Japon et en dépit de la grande quantité d’œuvres inspirées par ce pays dans sa production, il reste dans le peu de notoriété que lui accorde l’histoire de l’art exclusivement un peintre colonial. Fruit de recherches fondamentales au sujet d’un personnage très peu étudié, le présent mémoire s’appuie sur des documents d’époque mais aussi sur des sources primaires inédites dont la collection photographique de plus d’un millier de clichés acquis ou pris par Dumoulin et annotés de sa main, afin d’interroger le rôle qu'a joué l’Extrême-Orient et plus particulièrement le Japon dans la vie et l'œuvre de cet artiste et sa place dans le japonisme de la fin du XIXe siècle. Elle vise également, selon une approche imagologique et comparatiste avec des témoignages écrits et picturaux d’artistes, d’intellectuels et d’écrivains contemporains de Dumoulin, à analyser dans l’œuvre et les écrits de ce dernier les</td><td>["'", '(', ')', ',', '.', '1860-1924', '1888', '1897', 'Artiste', 'Asie', 'Chine', 'Dumoulin', 'Elle', 'Entre', 'Extrême-Orient', 'France', 'Fruit', 'Indochine', 'Japon', 'Jules', 'Louis', 'Malgré', 'Peintre', 'XIXe', 'a', 'accorde', 'acquis', 'afin', 'ainsi', 'analyser', 'annotés', 'approche', 'appuie', 'art', 'artiste', 'artistes', 'au', 'aussi', 'autres', 'avec', 'avoir', 'ce', 'certaine', 'ces', 'cet', 'clichés', 'collection', 'colonial', 'coloniale', 'colonialiste', 'comparatiste', 'conduiront', 'contemporains', 'convaincu', 'cours', 'd', 'dans', 'date', 'de', 'dernier', 'des', 'deux', 'documents', 'dont', 'du', 'dépit', 'effectue', 'effectué', 'en', 'et', 'exclusivement', 'exécutés', 'fait', 'fera', 'fin', 'fondamentales', 'fondateur', 'français', 'française', 'grande', 'histoire', 'il', 'imagologique', 'in', 'influence', 'inspirées', 'inspirés', 'intellectuels', 'interroger', 'inédites', 'japonisme', 'jouissant', 'joué', 'jusqu', 'l', "l'œuvre", 'la', 'le', 'les', 'livré', 'long', 'lui', 'main', 'mais', 'marine', 'militant', 'millier', 'missions', 'modèles', 'monde', 'mémoire', 'nombreux', 'notoriété', 'officiel', 'officielles', 'ou', 'par', 'parcouru', 'particulièrement', 'partir', 'patriote', 'pays', 'peintre', 'peintres', 'personnage', 'personnelle', 'peu', 'peuple', 'photographique', 'photographiques', 'picturaux', 'place', 'plus', 'plusieurs', 'politique', 'premier', 'premiers', 'primaires', 'pris', 'production', 'prolifique', 'présent', 'puisées', 'qu', 'quantité', 'que', 'qui', 'recherches', 'reprises', 'représentations', 'reste', 'riche', 'réception', 'rôle', 's', 'sa', 'selon', 'situ', 'siècle', 'sociale', 'société', 'soit', 'son', 'sources', 'sujet', 'sur', 'surtout', 'séjourner', 'tableaux', 'territoire', 'trois', 'très', 'témoignages', 'un', 'une', 'vie', 'vise', 'vivant', 'voyage', 'voyages', 'à', 'écrits', 'écrivains', 'également', 'époque', 'études', 'étudié', 'évolution', 'œuvre', 'œuvres', '’']</td><td valign="top">{"'": 0,<br>'(': 1,<br>')': 2,<br>',': 3,<br>'.': 4,<br>'1860-1924': 5,<br>'1888': 6,<br>'1897': 7,<br>'Artiste': 8,<br>'Asie': 9,<br>'Chine': 10,<br>'Dumoulin': 11,<br>'Elle': 12,<br>'Entre': 13,<br>'Extrême-Orient': 14,<br>'France': 15,<br>'Fruit': 16,<br>'Indochine': 17,<br>'Japon': 18,<br>'Jules': 19,<br>'Louis': 20,                                   ...                                        'société': 160,<br>'soit': 161,<br>'son': 162,<br>'sources': 163,<br>'sujet': 164,<br>'sur': 165,<br>'surtout': 166,<br>'séjourner': 167,<br>'tableaux': 168,<br>'territoire': 169,<br>'trois': 170,<br>'très': 171,<br>'témoignages': 172,<br>'un': 173,<br>'une': 174,<br>'vie': 175,<br>'vise': 176,<br>'vivant': 177,<br>'voyage': 178,<br>'voyages': 179,<br>'à': 180,<br>'écrits': 181,<br>'écrivains': 182,<br>'également': 183,<br>'époque': 184,<br>'études': 185,<br>'étudié': 186,<br>'évolution': 187,<br>'œuvre': 188,<br>'œuvres': 189}</td><td valign="top">{0: "'",<br>1: '(',<br>2: ')',<br>3: ',',<br>4: '.',<br>5: '1860-1924',<br>6: '1888',<br>7: '1897',<br>8: 'Artiste',<br>9: 'Asie',<br>10: 'Chine',<br>11: 'Dumoulin',<br>12: 'Elle',<br>13: 'Entre',<br>14: 'Extrême-Orient',<br>15: 'France',<br>16: 'Fruit',<br>17: 'Indochine',<br>18: 'Japon',<br>19: 'Jules',<br>20: 'Louis',                    ...                                160: 'société',<br>161: 'soit',<br>162: 'son',<br>163: 'sources',<br>164: 'sujet',<br>165: 'sur',<br>166: 'surtout',<br>167: 'séjourner',<br>168: 'tableaux',<br>169: 'territoire',<br>170: 'trois',<br>171: 'très',<br>172: 'témoignages',<br>173: 'un',<br>174: 'une',<br>175: 'vie',<br>176: 'vise',<br>177: 'vivant',<br>178: 'voyage',<br>179: 'voyages',<br>180: 'à',<br>181: 'écrits',<br>182: 'écrivains',<br>183: 'également',<br>184: 'époque',<br>185: 'études',<br>186: 'étudié',<br>187: 'évolution',<br>188: 'œuvre',<br>189: 'œuvres'}                       </td></tr></tbody></table>

Tokenisation pour "artiste colonial français" -> \[34, 47, 77]

Tokenisation pour "Dumoulin" -> \[11]

## Principe de l'attention

L' Attention consiste à établir le degré d'importance que chaque token doit accorder aux autres tokens, de manière à ce que chaque token se voit attribuer un score par rapport à tous les autres dans une séquence donnée.

Todo: synthétiser le principe de l'attention décrit dans le post:

{% embed url="https://pub.towardsai.net/no-libraries-no-shortcuts-llm-from-scratch-with-pytorch-664c557997ee" %}

## Schéma général d'un Transformer : une architecture encoder-decoder

<figure><img src="../.gitbook/assets/Capture d&#x27;écran 2025-09-24 181217 (1).png" alt=""><figcaption></figcaption></figure>

## Partie Encodeur (modèles d'embeddings)

_L’encodeur lit et comprend le texte :_ l'encodeur est une pile de sous-couches (de neurones) chargée de l'encodage : la génération des embeddings contextualisés qui porte la sémantique du texte.

Les embeddings résultent d'un encodage bi-directionnel + encodage positionnel : ils sont calculés avec une self-attention bidirectionnelle qui fait que durant l'apprentissage chaque token "voit" tous les tokens de la séquence entière, à droite et à gauche et dans l'ordre

## **Partie décodeur (modèels génératifs)**

_Le décodeur écrit_ : le décodeur est une pile de sous-couches (de neurones) chargée de générer le prochain token.

Il prend en entrée les embeddings en sortie de l'encodeur et créent de nouveaux embeddings issus de la self-attention causale qui fait que durant l'apprentissage chaque token ne voit que les tokens qui le précèdent.

Principe du masque triangulaire





Chaque couche construit des représentations de plus en plus complexes

