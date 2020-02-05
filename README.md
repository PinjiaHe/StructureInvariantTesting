# SIT: Structure-Invariant Testing for Machine Translation

Translation errors found by our SIT approach in [Google Translate](https://translate.google.com/#view=home&op=translate&sl=en&tl=zh-CN) and [Bing Microsoft Translator](https://www.bing.com/translator). 

SIT is capable of finding translation errors of diverse kinds. In our experiments, we mainly find 5 kinds of translation errors: under-translation, over-translation, incorrect modification, word/phrase mistranslation, and unclear logic. We will keep updating the erroneous translations uncovered by SIT to this repo.


### Translation error categories:
+ **Under-translation**: If some words are mistakenly untranslated (i.e. do not appear in the translation), it is an under-translation error.
+ **Over-translation**: If some words are unnecessarily translated multiple times or some words in the target sentence are not translated from any words in the source sentence, it is an over-translation error.
+ **Incorrect modification**: If some modifiers modify the wrong element in the sentence, it is an incorrect modification error.
+ **Word/phrase mistranslation**: If some tokens or phrases are incorrectly translated in the target sentence, it is a word/phrase mistranslation error.
+ **Unclear logic**: If all the tokens/phrases are correctly translated but the sentence logic is incorrect, it is an unclear logic error.

### Template in the following examples: :telescope: 
+ Row 1: error category
+ Row 2: source sentence (in English)
+ Row 3: target sentence (in Chinese)
+ Row 4: target sentence meaning


### Input and output example
The input of SIT is a list of unlabeled sentences in source language. For clarity, we use an unlabeled sentence here as an example.


||
| :------------- | 
| I had a story to tell and I wanted to finish it, Draper says. | 
||

The output of SIT is a list of issues. The meanings of key terminologies used in the paper are as follows:
+ **Translation error**: Mistranslation of some parts of a source sentence. A translated sentence could contain more than one translation errors.
+ **Sentence pair**: A source sentence and its corresponding target sentence returned by the machine translation software.
+ **Erroneous sentence**: A target sentence that contains at least one translation error. We use "error in the target sentence" and "error in the sentence pair" interchangeably.
+ **Issue and erroneous issue**: An issue contains (1) the original source sentence and its translation; and (2) top-k generated source sentences and their translations, which SIT thinks are erroneous. An issue is erroneous if (1) the translation of the original sentence is erroneous; or (2) the translation of at least one reported generated source sentences is erroneous.

An example of reported issue (top-3 sentence pairs) by SIT is as follows. Note the error types are also provided here for explanation.

| original: correct |
| :------------- | 
|I had a story to tell and I wanted to finish it, Draper says.|
|德雷珀说，我有一个要讲的故事，我想完成它。| 
|I had a story to tell and I wanted to finish it, Draper says.| 

| 1st: word/phrase \| over |
| :------------- | 
|I had a part to tell and I wanted to finish it, Draper says.|
|德雷珀说，我有责任告诉我，我想完成它。| 
|I had the responsibility to tell myself that I wanted to finish it, Draper says.|

+ _word/phrase_: "part" is mistranslated to "responsibility".
+ _over_: "myself" appears in the target sentence but not in the source sentence.

| 2nd: logic |
| :------------- | 
|I had a joke to tell and I wanted to finish it, Draper says. |
|德雷珀说，我开玩笑说，我想完成它。| 
|I joked that I wanted to finish it, Draper says.|
+ _logic_: should be "had a joke to tell", not "joked that".

| 3rd: correct |
| :------------- | 
|I had a lot to tell and I wanted to finish it, Draper says.|
|德雷珀说，我有很多要说的，我想完成它。| 
|I had a lot to tell and I wanted to finish it, Draper says.| 

As we can see, this issue contains 3 reported sentences/sentence pairs, while 2 of them are erroneous sentences. The first erroneous sentence contains 2 translation errors: _word/phrase mistranslation_ and _over-translation_. The second erroneous sentence contains 1 translation error: _unclear logic_. Since this issue contains erroneous sentences, it is a erroneous issue.

### Unique errors

In Table 3 of the paper, we demonstrate the number of _unique errors_ reported. In the same issue, we regard a error as a unique error if either:
+ the error type is different from other errors in the same issue; or
+ the mistranslation part is different.

For example, two erroneous sentences pairs that contain under-translation errors are as follows.

| under |
| :------------- | 
|Young people of color became a majority of K 12 public school students in 2014.|
|2014年，有色人种成为K 12公立学校学生的大多数。| 
|People of color became a majority of K 12 public school students in 2014.|

| under |
| :------------- | 
|Young people of color became a minority of K 12 public school students in 2014.|
|2014年，有色人种成为K 12名公立学校学生中的少数。| 
|People of color became a minority of K 12 public school students in 2014.| 

They contain 1 unique error, because the errors are of the same type and both of them do not translate "Young" in the target sentence.

### Examples of translation errors detected by SIT

### Google Translate

| under |
| :--- |
|The former employee is now looking for new career opportunities outside their field of expertise after hitting a road block in the job search.|
|这位前员工在求职时遇到障碍后，正在寻找新的职业机会。|
|The former employee is now looking for new career opportunities after hitting a road block in the job search.|

| under \| over |
| :--- |
|The investigators were right that the airplane itself was safe.|
|调查人员认为飞机本身是安全的。|
|The investigators thought that the airplane itself was safe.|

| word/phrase \| logic \| over |
| :--- |
|Economists can come close to an answer, enabling the company to make more decisions about which benefits to include and which to scrap.|
|经济学家可以接近答案，使公司能够做出更多决定，包括哪些利益包括哪些利益，哪些利益要废弃。|
|Economists can come close to an answer, enabling the company to make more decisions, which benefits to include which benefits to include and which to get rid of (an old vehicle).|

| modification \| word/phrase |
| :--- |
|The South has emerged as a hub of new auto manufacturing by foreign makers thanks to lower manufacturing costs and less powerful businesses.|
|由于制造成本降低和业务不那么强大，南方已成为外国制造商新的汽车制造中心。|
|The South has emerged as a new hub of auto manufacturing by foreign makers thanks to the reducing manufacturing costs and less powerful businesses.|

| word/phrase |
| :--- |
|The most elite public universities admit a considerably larger percentage of students from lower income backgrounds than do the elite private schools.|
|最精英的公立大学承认，与精英私立学校相比，低收入学生的比例要高得多。|
|The most elite public universities agree unwillingly that considerably larger percentage of students from lower income backgrounds than do the elite private schools.|

| word/phrase |
| :--- |
|It declined to ground the jet.|
|它拒绝接近喷气式飞机。|
|It declined to get close to the jet.|

| logic |
| :--- |
|And attacking a dead man who spent five years as a prisoner of war and another three decades serving the country in elected office, is simply wrong.|
|并且攻击一名死去的人，他在战争中担任战争囚犯五年，另外三十年担任民选职务的国家，这是完全错误的。|
|And attacking a dead man who spent five years as a prisoner of war and another three decades serving in elected office as a country, is simply wrong.|

:point_right: More translation errors in Google Translate detected by SIT can be found [here](errorsgoogle.md).

### Bing Microsoft Translator


| under |
| :--- |
|After pleading guilty in the Manhattan probe, Cohen also later pleaded guilty to lying to Congress in a case brought by Mueller's website.|
|在曼哈顿调查中认罪后，科恩后来还对穆勒网站提起的一起案件中的撒谎罪供认不讳。|
|After pleading guilty in the Manhattan probe, Cohen also later pleaded guilty to lying in a case brought by Mueller's website.|

| over |
| :--- |
|I am very happy to share my point of view.|
|我很高兴与大家分享我的观点。|
|I am very happy to share my point of views with everyone.|

| modification |
| :--- |
|Anxious gossip about who is and is not mentioned in the latest news reports.|
|关于最新新闻报道中谁是谁和谁没有被提及的焦虑流言蜚语。|
|Anxious gossip about who is who and who is not mentioned in the latest news reports.|

| word/phrase |
| :--- |
|But it would keep the new investment in Michigan and focus there on self driving vehicles, according to reports.|
|但据报道，这将保留对密歇根州的新投资，并将重点放在自驾游车辆上|
|But it would keep the new investment in Michigan and focus there on vehicles for self driving tours, according to reports.|

| word/phrase |
| :--- |
|The South has emerged as a hub of new auto manufacturing by foreign makers thanks to lower manufacturing costs and less powerful unions.|
|由于制造成本较低，工会实力较弱，韩国已成为外国制造商新汽车制造业的枢纽。|
|The South Korea has emerged as a hub of new auto manufacturing by foreign makers thanks to lower manufacturing costs and less powerful unions.|

| logic |
| :--- |
|Since then, the ex employee has been rejected for multiple jobs and is still searching.|
|此后，前员工因多个工作被拒绝，目前仍在寻找中。|
|Since then, because the ex employee has been rejected for multiple jobs, he is still searching.|

| under |
| :--- |
|During a question and answer session, however, Nielsen added that other nation states have adopted a more visible approach to doing the same.|
|不过，在问答环节，尼尔森补充说，其他民族国家也采取了更明显的做法。|
|During a question and answer session, however, Nielsen added that other nation states have adopted a more visible approach.|

| over |
| :--- |
|Entering talks, Brazil hoped to see its elf elev ated to major non NATO ally status by the Trump administration, a big step that would help it purchase military equipment.|
|进入谈判，巴西希望看到自己被特朗普政府提升为主要的非北约盟国地位，这是一个帮助其购买军事装备的一大步。 |
|Entering talks, Brazil hoped to see its elf elev ated to major non NATO ally status by the Trump administration, one a big step that would help it purchase military equipment.|

| word/phrase |
| :--- |
|I am very willing to share my point of view.|
|我非常愿意同意我的观点。|
|I am very willing to agree with my point of view.|

| word/phrase \| logic \| over|
| :--- |
|Covering a memorial service in the nation's capital and then traveling to Texas for another service as well as a funeral train was an honor, he says.|
|他说，在美国首都举行追悼会，然后前往德州参加另一次礼拜仪式以及葬礼列车，是一种荣誉。|
|Holding a memorial service in the nation's capital and then traveling to Texas for attending another church service and a funeral train was an honor, he says.|


:point_right: More translation errors in Bing Microsoft Translator detected by SIT can be found [here](errorsbing.md).

