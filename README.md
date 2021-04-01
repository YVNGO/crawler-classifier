# crawler-classifier
Background
Sentiment analysis, which is a powerful technique based on natural language processing, has a wide range of applications, including consumer reviews analysis, recommender system, political campaigning, stock speculation, etc. A sentiment analysis model requires a large text corpus, which consists of classified articles grabbed from the internet using web crawlers.
In the simplest scenario, a text corpus can be built by two components: a web crawler and a classifier. The crawler browses through web pages and grabs articles from websites. The grabbed articles are stored in a buffer, from which the classifier processes articles and classifies them.
Considering the complexity of modern websites, it usually takes a long time for a crawler to locate and grab an article from the web page. So, the speed of crawlers is usually too slow for the classifier. Thus, multiple crawlers would be a better choice.


crawler
Each crawler thread is created to grab articles from websites and load them into the buffer. It keeps doing grabbing and loading job, which takes time interval_A, until the buffer is full. And then it starts waiting until the classifier deletes an article from the buffer.
A function char* str_generator(void), is provided to generate articles for the crawler to grab and each article is represented by a string of 50 characters.

buffer
The buffer structure is a first-in-first-out (FIFO) queue. It is used to store the grabbed articles from crawlers temporarily, until they are taken by the classifier. It can store up to 12 articles at the same time.

classifier
A classifier thread is created to classify the articles grabbed by the crawlers in FIFO order.
Specifically, there are two steps in the procedure:
1. Pre-processing: the classifier makes a copy of the article at the head of the buffer, changes
all the uppercase letter (‘A’-‘Z’) to lowercase letter (‘a’-‘z’) and deletes any symbol that is not
a letter.
2. Classification: the classifier classifies the article into one of the 13 classes based on the
first letter, x, of the processed article as follows.
Class label = int(x – ‘a’)%13 + 1
Next, an auto-increasing key starting from 1 will be given to the classified article. (So, the
keys of classified articles are 1, 2, 3, …). At last, the key, the class label and the original
article, are stored to the text corpus in a text file. Then, the classifier deletes the classified
article in the buffer. The whole procedure takes time of interval_B.

termination
The articles are divided into 13 classes. Denote the number of articles in each class as C1, C2,
… C13, and p = min{ C1, C2, … C13}. When p ≥ 5, the classifier notifies all crawlers to quit after
finishing the current job at hand, and then the program terminates.

input arguments
program acceptS the following two arguments in input order:
interval_A, interval_B: integer, unit: microsecond.

sample outputs
The outputs of your program are:
• A table with multiple columns shown on the screen, each column shows the
activities of a single thread in time order, and each row shows only one single
activity of a thread.
• The text corpus, each line consists of a key, a class label and an article separated by a
space.
All activities that need to be recorded for each thread are listed below, together with their
abbreviations.
Crawler:
start – crawler starts.
grab – crawler starts to grab an article.
f-grab – an article has been grabbed and loaded into the buffer.
wait – crawler starts waiting for available space in the buffer.
s-wait – crawler stops waiting.
quit – crawler finished all job and about to quit.
Classifier:
start – classifier starts.
clfy – classifier starts to classify an article.
f-clfy – the article has been classified and deleted from the buffer.
k-enough – k number of articles have been classified and the classifier notifies all
threads to quit.
3
n-stored – a total n articles have been stored in the text corpus.
quit – classifier finished all job and about to quit.


Most modern websites are under anti-crawler protection. Thus, crawlers should be updated with new IP addresses and cookies periodically to get through the barrier.
A strategy manager thread is created to update the crawlers with a new IP and cookies. Each crawler notifies the strategy manager to update its IP and cookies after every M articles are grabbed. The update takes time of interval_C. The input and extra output are listed below.
Program has to accept the following arguments in input order:
interval_A, interval_B, interval_C: integer, unit: microsecond, M: integer.
Crawler: two more activities have to be recorded:
rest – crawler starts resting.
s-rest – crawler stops resting.
Strategy-Manager:
start – manager starts.
get-crx – manager gets a notification from crawler x.
up-crx – manager updated crawler x with new IP and cookies.
quit – manager finished all job and about to quit.


generator.cpp
The function char* str_generator(void) is provided in the file generator.cpp. It returns a string (char array) of length 50. Use it by declaring a prototype in your code and compiling it along with your source code.
