# ULMFiT Language Model for Malay Language

State-of-the-Art Language Modeling in Malay language with perplexity of 29.30245 on Malay Wikipedia.

## The Project

This directory contains relevant files for implementating ULMFiT language model by Jeremy Howard (fast.ai) and Sebastian Ruder applied to NLP tasks for the Malay language using the [Malay Wikipedia](https://ms.wikipedia.org) corpus.

---

### Background

_This work is part of my project while studying [fast.ai's 2018 edition of Cutting Edge Deep Learning for Coders, Part 2](http://course.fast.ai/part2.html) course._

I took this opportunity to implement [Universal Language Model Fine-tuning for Text Classification (ULMFiT) paper](http://nlp.fast.ai/classification/2018/05/15/introducting-ulmfit.html) in different languages together with the fast.ai community. fast.ai will soon launch a [model zoo with pre-trained language models for many languages](http://forums.fast.ai/t/language-model-zoo-gorilla/14623). You can learn more about ULMFiT in lesson 4 and lesson 10. What I learned from lesson 10 ([my notes](https://cedrickchee.gitbook.io/knowledge/courses/fast.ai/deep-learning-part-2-cutting-edge-deep-learning-for-coders/2018-edition/lesson-10-transfer-learning-nlp)) is:
- how pre-training a full language model from scratch can greatly surpass previous approaches based on simple word vectors
- transfer learning for NLP by using this language model to show a new state of the art result in text classification, in some sense like [NLP's ImageNet moment has arrived](http://ruder.io/nlp-imagenet/)

### Project Goal

The goal of this project is to train Malay word embeddings using the fast.ai version of [AWD-LSTM Language Model](https://arxiv.org/abs/1708.02182) by Salesforce Research—basically LSTM with dropouts—with data from [Wikipedia](https://dumps.wikimedia.org/mswiki/20180901/mswiki-20180901-pages-articles.xml.bz2) (last updated Sept 2, 2018). The AWD-LSTM language model achieved the state of the art performance on the English language.

Using 90/10 train-validation split, I achieved perplexity of **29.30245 with 60,002 embeddings at 400 dimensions**, compared to state-of-the-art as of June 12, 2018 at **40.68 for English WikiText-2 by [Yang et al (2017)](https://arxiv.org/abs/1711.03953)** and **29.2 for English WikiText-103 by [Rae et al (2018)](https://arxiv.org/abs/1803.10049)**. To the best of my knowledge, there is no comparable research in Malay language at the point of writing (Sept 21, 2018).

My workflow is as follows:
- Perform 90/10 train-validation split
- Minimal text cleaning and tokenization using our own tokenizer
- Train language model
- Evaluate model based on perplexity and eyeballing
- Get embeddings of dataset from train set

For the community to reuse the model directly, I am contributing the Jupyter notebook (and code) together with the pre-trained weights to the fast.ai model zoo.

Due to some challenges to find curated and publicly available dataset for Malay text, I can't provide a benchmark for text classification yet, but as soon as I can find one (please contact me if you have one), I will update my research.

The language model can also be used to extract text features for other downstream tasks.

## Dependencies

- Python 3+ (tested with 3.6.5)
- PyTorch 0.4+ (tested with 0.4.0)
- fast.ai (pip install git+https://github.com/fastai/fastai.git@master#egg=fastai)

## Version History

### v0.1

- Pretrained language model based on Malay Wikipedia with the perplexity of 29.30245.

## Pre-trained model

You can download the files from [Google Drive](https://drive.google.com/drive/folders/1p5fsrD97iRD-Vz6C_ae5fo4c5wY0KrJd?usp=sharing):
- Weights for the pre-trained model (lm_malay_final.h5.tar.gz)
    - Uncompress and put the weights (.h5 file) into `{project_root}/data/models/`.
- Index-to-word mapping [itos (index -> string) mapping] (itos.pkl.tar.gz)
    - Uncompress and put the pickled objects (.pkl files) into `{project_root}/data/model/malay/tmp/`.
- Pre-processed training dataset of Malay Wikipedia:
    - tokenized training text data (tok_trn.npy.tar.gz)
    - tokenized validation text data (tok_val.npy.tar.gz)
    - indexed representation of train set (trn_ids.npy.tar.gz)
    - indexed representation of validation set (val_ids.npy.tar.gz)
    - Uncompress and put the numpy array binary (.npy files) into `{project_root}/data/model/malay/tmp/`.

The weights (model state dict) and the optimizer state for the model were saved at the end of the training.

_Note: the model was last trained on 2018-09-22 and the weights last updated on 2018-09-22._


## Inference

### Test 1: Generate text using the language model

Generate sentences using some random strings. Examples:
- "Menara Petronas"
  - menara petronas. pada tahun 2005, sebuah syarikat yang dikenali sebagai Petronas, yang dimiliki oleh Petronas, telah membeli sebuah syarikat yang dimiliki oleh Petronas, Petronas. pada tahun 2008, Petronas membeli saham mereka daripada Petronas, dan Petronas, dan Petronas, dan Petronas, dan Petronas, dan Petronas, dan Petronas, yang telah membeli saham mereka pada tahun 2008.
- "Dr Mahathir merupakan Perdana Menteri"
  - dr mahathir merupakan perdana menteri malaysia yang pertama. beliau juga merupakan seorang ahli parlimen bagi kawasan DUN N53. beliau juga merupakan ahli parlimen bagi kawasan DUN N53. beliau juga merupakan ahli parlimen bagi kawasan DUN N53. beliau juga merupakan ahli parlimen bagi kawasan DUN N53. beliau juga merupakan ahli parlimen bagi kawasan DUN N53. beliau juga merupakan ahli parlimen bagi kawasan bukit gelugor, negeri sembilan. beliau juga
- "Tunku ialah Bapa Kemerdekaan"
  - tunku ialah bapa kemerdekaan malaysia. beliau juga merupakan seorang ahli politik yang pernah berkhidmat sebagai perdana menteri malaysia ke-6. beliau juga merupakan seorang ahli parlimen bagi kawasan bukit besi, malaysia. beliau juga merupakan ahli parlimen bagi kawasan bukit gelugor, pulau pinang. beliau juga merupakan ahli parlimen bagi kawasan bukit gelugor, pulau pinang. beliau juga merupakan ahli parlimen bagi kawasan bukit gelugor, pulau pinang. beliau juga merupakan ahli parlimen bagi kawasan bukit gelugor, pulau
- "Syarikat penerbangan"
  - syarikat penerbangan ini. pada tahun 2005, sebuah syarikat penerbangan antarabangsa, BAE systems, telah ditubuhkan. pada tahun 2005, syarikat penerbangan ini telah melancarkan operasi untuk melancarkan pesawat pejuang generasi baru, "ATR 72". pada tahun 2009, syarikat penerbangan ini telah melancarkan operasi untuk melancarkan pesawat pejuang generasi baru, "ATR 72". pada tahun 2014, syarikat penerbangan ini telah melancarkan operasi untuk melancarkan pesawat pejuang generasi
- "Durian ialah buah"
  - durian ialah buah - buahan yang ditanam di dalam hutan.
- "P Ramlee ialah seorang"
  - p ramlee ialah seorang ahli perniagaan yang berjaya. beliau juga merupakan seorang ahli perniagaan yang berjaya. beliau juga merupakan seorang ahli perniagaan yang berjaya. beliau juga merupakan seorang ahli perniagaan dan ahli perniagaan. beliau juga merupakan seorang ahli perniagaan dan ahli perniagaan. beliau juga merupakan seorang ahli perniagaan dan ahli perniagaan. beliau juga merupakan seorang ahli perniagaan dan ahli perniagaan. beliau juga merupakan seorang ahli perniagaan dan ahli perniagaan. beliau juga merupakan seorang ahli perniagaan dan
- "Pemenang badminton Piala Thomas"
  - pemenang badminton piala thomas, PBB. pada tahun 2005, persatuan bola sepak malaysia (FAM) mengumumkan bahawa FAM akan menubuhkan persatuan bola sepak malaysia (FAM). pada tahun 2009, FAM mengumumkan bahawa FAM akan menyertai AFF pada tahun 2013. FAM juga telah mencadangkan bahawa FAM akan menyertai AFF pada tahun 2013. FAM juga telah bersetuju untuk menyertai AFF pada tahun 2014.
- "Lee Chong Wei dan badminton"
  - lee chong wei dan badminton. pada tahun 2005, sebuah lagi acara sukan diadakan di stadium nasional bukit jalil, kuala lumpur. pada tahun 2009, persatuan bola sepak malaysia (FAM) telah mengadakan perlawanan persahabatan menentang (FAM), (FAM), (FAM), (FAM), (FAM), (FAM), (FAM), AFC dan FIFA. pada tahun 2009, FAM mengumumkan bahawa mereka akan bertanding dalam piala malaysia pada tahun
- "Jurulatih Rashid Sidek"
  - jurulatih rashid sidek, seorang ahli politik yang telah berkhidmat sebagai perdana menteri malaysia. beliau juga merupakan seorang ahli parlimen bagi kawasan DUN N53. beliau juga merupakan ahli parlimen bagi kawasan bukit gelugor, negeri sembilan. beliau juga merupakan ahli parlimen bagi kawasan bukit gelugor, negeri sembilan. beliau juga merupakan ahli parlimen bagi kawasan bukit gelugor, negeri sembilan. beliau juga merupakan ahli parlimen bagi kawasan bukit gelugor, negeri sembilan. beliau juga merupakan
- "Pokok getah"
  - pokok getah. pada tahun 2011, sebuah lagi projek baru yang dikenali sebagai "the new york times" telah dilancarkan. pada tahun 2011, sebuah lagi projek baru yang dikenali sebagai "the new york times" telah dilancarkan. pada tahun 2013, sebuah siri "the last airbender" telah diterbitkan semula sebagai siri "the last airbender". pada tahun 2013, "the new york times" melaporkan bahawa "the new york
- "Industri kelapa sawit di Malaysia"
  - industri kelapa sawit di malaysia. pada tahun 2005, sebuah syarikat yang dikenali sebagai Petronas, telah ditubuhkan untuk membangunkan dan memajukan Petronas sebagai sebuah syarikat yang bertanggungjawab untuk pembangunan dan pembangunan teknologi maklumat. Petronas telah ditubuhkan pada tahun 1990, dan pada tahun 1992, Petronas telah menjadi syarikat yang pertama untuk membangunkan dan menghasilkan produk - produk yang berkualiti. Petronas telah membeli dan membeli sebuah syarikat yang dikenali sebagai Petronas Petronas,
- "Penyelidikan minyak sawit"
  - penyelidikan minyak sawit, dan juga beberapa jenis produk yang berkaitan dengan industri.
- "Negara terbesar di Asia Tenggara ialah"
  - negara terbesar di asia tenggara ialah UNESCO. sejarah. pada tahun 2011, sebuah lagi muzium sejarah di indonesia, iaitu muzium negara indonesia, telah dibuka di jakarta, indonesia. muzium ini telah dirasmikan oleh YAB perdana menteri malaysia, tun dr. mahathir bin mohamad pada 1 jun 2007. muzium ini mempamerkan koleksi seni bina yang unik dan menarik. muzium ini mempamerkan koleksi seni bina yang unik dan menarik. muzium ini mempamerkan koleksi seni bina
- "Proton Saga adalah"
  - proton saga adalah sebuah kereta kebal utama yang digunakan oleh tentera udara diraja malaysia.
- "Penyanyi terkenal"
  - penyanyi terkenal, penyanyi, penyanyi, penulis lagu, komposer, komposer, komposer, komposer, dan artis. lagu. lagu ini digubah oleh komposer terkenal, komposer terkenal, komposer terkenal, ahmad nawab. lagu ini digubah oleh ahmad nawab, dan liriknya ditulis oleh ahmad nawab. lagu ini digubah oleh ahmad nawab, dan liriknya ditulis oleh ahmad nawab. lagu ini digubah oleh ahmad nawab, dan dinyanyikan oleh p. ramlee.

## Text Classification

I am still trying to find curated or publicly available dataset for Malay corpus. Therefore, I can't provide a benchmark for text classification yet.

## TODO

- [ ] Finetune language model for text classification task
- [ ] Use continuous cache pointer (from here: https://github.com/salesforce/awd-lstm-lm)
- [ ] Try QRNN (from here: https://github.com/salesforce/pytorch-qrnn/)
- [ ] Identify new datasets for sentiment analysis

## License

MIT. Copyright 2018 Cedric Chee.