# Day 26 - April 2

## Amazon Rekognition
- Find objects, people, text, scenes in images and videos using ML
- facial analysis and facial search to do user verification, people counting
- create a database of "familiar faces" or compare against celebrities
- Use Cases:
  - Labelling
  - content moderation
  - text detection
  - face search and verification
  - celebrity recognition
  - Pathing
## Amazon rekognition - content moderation
- Detect content that is inappropriate, unwanted, or offensive (images and videos)
- used in social media, broadcast media, advertising and e-commerce situations to create a safer user experience
- set a minimum confidence threshold for items that will be flagged
- flag sensitive content for manual review in augmented AI (A2I)
- help comply with regulations
## Amazon Transcribe
- Automatically convert speech to text
- Uses deep learning process called automatic speech recognition (asr) to convert speech to text quickly and accurately
- automatically remove personally identifiable information (PII) using redaction supports automatic language identification for multi lingual audio
- use cases: transcribe customer service calls
- automate closed captioning and subtitling
- generate metadata for media assets to create a fully searchable archive
## Amazon Polly
- Turn text into lifelike speech
- Uses deep learning to synthesize natural-sounding speech from text
- Supports multiple languages, voices, and speech styles
- use cases: create interactive voice applications, automated phone systems, audiobooks, and accessibility solutions
- enables developers to add speech output to applications without requiring audio recording
## Amazon Polly - lexicon and ssml
- customize the pronounciation of words with pronounciation lexicons
  - stylized words: st3ph4ane => "Stephane"
  - acronyms: AWS => "amazon web services"
- Upload the lexicons and use them in the synthesizespeech operation
- generate a speech from plain text or from documents marked up with speech synthesis markup language (ssml) - enables more customization
  - emphasizing speecific words or phrases
  - using phonetic pronounciation
  - including breathing sounds, whispering
  - using the newscaster speaking style
## Amazon Translate
- Natural and accurate language translation
- amazon translate allows you to localize content - such as websites and applications for international users and to easily translate large volumes of text efficiently
## Amazon Lex & Connect
- amazon lex: (same tech that powers alexa)
  - auto speech recognition (asr)to convert speech to text
  - natural language understanding to recognize the intent of text, callers
  - helps build chatbots, call center bots
- amazon connect: 
- receive calls, create contact flows, cloud baed virtual contact center
- can integrate with other CRM systems over AWS
- No upfront payments, 80% cheaper than traditional contact center solutions
## Amazon comprehend
- for natural language processing (NLP)
- fully managed and serverless service
- uses machine learning to find insights and relationships in text
  - language of the text
  - extracts key phrases, places, people, brands, or events
  - understand how positive or negative the text is
  - analyzes text using tokenization and parts of speech
  - automatically organizes a collection of tet files by topic
  - sample use cases:
    - analyze customer interactions(emails): to find what leads to a positive or negative experience
    - create and groups articile by topics that comprehend will uncover
## Amazon Comprehend Medical
- Amazon comprehend medical detects and returns useful information in unstructured clinical text:
  - physician's notes
  - discharge summaries
  - test results
  - case notes
- Uses NLP to detect protected health information (PHI) - DetectPHI API
- Store your documents in Amazon s3, analyze realtime data with kinesis data firehose or use amazon transcribe to transcribe patients narratives into text that can be analyzed by amazon comprehend medical
## Amazon Sagemaker AI
- fully managed service for developers/data scientists to build ML modeles
- typically difficult to do all the processes in one place + provision servers
- machine learning process (simplified): predicting your exam score
## Amazon kendra
- fully managed document search service powered by machine learning
- extract answers from within a document (text, pdf, html,powerpoint, ms word, faqs)
- natural language search capabilities
- learn from user interactions/feedback to promote preferred results (incremental learning)
- Ability to manually fine-tune results (importance of data, frensess, custom,...)
## Amazon Personalize
- Fully managed ML-service to build apps with real-time personalized recommendations
- Same technology used by amazon.com
- integrates into existing websties, applications, sms, email marketing systems, ...
- implement in dsys not months ( you dont need to build, train and deploy ml solutions)
- use cases: retail stores, media and entertainment...
## Amazon Extract
- automatically extracts texts, handwriting, and data from any scanned documents using ai and ml
- extract data from forms and tables
- read and process any types of documents (pdfs, images..)
- use cases:
  - financial services
  - healthcare
  - public sector
## AWS machine learning summary
- Rekognition: face detection, labeling, celebrity recognition
- transcribe: audio to text
- polly: text to audio
- translate: translations
- lex: build conversational bots - chatbots
- connect: cloud contact center
- comprehend: natural language processing
- sagemaker: machine learning for every developer and data scientist
- kendra: ML powered search engine
- personalize: real-time personalized recommendations
- textract: detect text and data in documents
