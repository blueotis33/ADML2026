0. START HERE: Load the CSV and do the first clean-up
0.1 Load the CSV
python
import pandas as pd                 # pandas = the tool for working with tables

df = pd.read_csv('xxxx.csv')        # swap xxxx for your file name. df = your table
print(df.head())                    # show the first 5 rows to check it worked

Where the file goes: put the CSV in the same folder as your .py file, by dragging it into PyCharm's project panel. Then you only need the file name.

If it's somewhere else, use the full path with an r in front:

python
df = pd.read_csv(r'C:\Users\c3688328\Downloads\xxxx.csv')

(Right-click the file in File Explorer → Copy as path.)

If it goes wrong:

Problem	Fix
FileNotFoundError	Check the spelling and .csv, or use the full path
UnicodeDecodeError	pd.read_csv('xxxx.csv', encoding='latin1')
Everything is in one column	pd.read_csv('xxxx.csv', sep=';')
It's an Excel file	pd.read_excel('xxxx.xlsx'), after python -m pip install openpyxl
0.2 Look at the data first (always do this)
python
print(df.shape)              # (number of rows, number of columns)
print(df.info())             # column names, types, missing values
print(df.describe())         # min / max / average: spot weird numbers
print(df.isnull().sum())     # how many gaps in each column
print(df.duplicated().sum()) # how many repeated rows

📸 Screenshot these outputs for the report. They prove you checked the data.

0.3 Pre-processing techniques: what they do and when to use them
Technique	What it does (simple)	When to use it	Code
Remove duplicates	Deletes rows that are exact copies	df.duplicated().sum() is more than 0	df = df.drop_duplicates()
Drop missing rows	Deletes rows with gaps	Only a few rows have gaps (under about 5%)	df = df.dropna()
Fill with median	Fills number gaps with the middle value	Number column with gaps, especially if it has outliers	df['Age'] = df['Age'].fillna(df['Age'].median())
Fill with mean	Fills number gaps with the average	Number column with gaps and no big outliers	df['Age'] = df['Age'].fillna(df['Age'].mean())
Fill with mode	Fills text gaps with the most common value	Text/category column with gaps	df['City'] = df['City'].fillna(df['City'].mode()[0])
Drop columns	Removes columns that don't help	ID numbers, names, or a column that's mostly empty	df = df.drop(columns=['CustomerID'])
Fix data types	Turns text that should be numbers into numbers	A number column shows as object in df.info()	df['Price'] = pd.to_numeric(df['Price'], errors='coerce')
Tidy text	Makes "Male", " male" and "MALE" all the same	Category columns with messy spelling or spaces	df['Gender'] = df['Gender'].str.strip().str.lower()
Remove outliers (IQR)	Deletes crazy extreme values	describe() shows a max that's far too big (e.g. age 999)	See the cleaning steps in section 4
Label encoding	Turns categories into numbers, e.g. Yes/No → 1/0	The target column (what you're predicting), or yes/no columns	from sklearn.preprocessing import LabelEncoder
df['Churn'] = LabelEncoder().fit_transform(df['Churn'])
One-hot encoding	Makes a 0/1 column for each category	Text feature columns with several options (e.g. City)	df = pd.get_dummies(df, columns=['City'])
Split X and y	Separates the inputs from the answer	Classification or regression (not clustering)	X = df.drop(columns=['Churn'])
y = df['Churn']
Train/test split	Keeps some data back to test the model fairly	Classification or regression	from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
Scaling (StandardScaler)	Puts all numbers on the same scale so big numbers don't dominate	KNN, SVM, logistic regression, K-Means, DBSCAN, PCA. Not needed for trees or random forests	from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)
Check balance	Shows if one answer is much more common	Always, for classification	print(y.value_counts())
Text cleaning (Week 6)	Removes HTML, punctuation and filler words, and shortens words	The data is text (reviews, tweets)	BeautifulSoup, re, stopwords, WordNetLemmatizer
Vectorising text (Week 6)	Turns words into numbers	After cleaning text, before the model	from sklearn.feature_extraction.text import TfidfVectorizer
X = TfidfVectorizer().fit_transform(df['text'])
One-hot baskets (Week 4)	Turns shopping data into True/False per item	Using apriori / association rules	Each column is an item and each row is a basket, with True/False values
0.4 The usual order
Load (read_csv)
Look (head, info, describe, isnull)
Remove duplicates
Fix missing values
Drop useless columns
Fix types and messy text
Handle outliers
Encode categories into numbers
Split X / y, then train / test (skip for clustering)
Scale (if the model needs it)
Build the model

💬 Report tip: for every step, write what you did and why. For example: "I filled missing ages with the median because the column had outliers, and the median isn't affected by them."

A plain-English guide to my tutorials: which packages to install for which model, and how to clean data step by step.

Golden rule: in the PyCharm Terminal, always install with python -m pip install <package> (not just pip install). This puts the package in the same Python your Run button uses.

1. Setup: installing packages
1.1 Check which Python you are using
python --version

Python 3.12 is the safest. Python 3.14 is very new and some packages (like fuzzy-c-means) may break.

1.2 Install everything at once (easiest)
python -m pip install numpy pandas matplotlib seaborn scikit-learn fuzzy-c-means mlxtend nltk beautifulsoup4 wordcloud
1.3 Check it worked
python -c "import numpy, pandas, sklearn; print('all good', numpy.__version__)"

NumPy should be version 2.x. If you see OverflowError: cannot convert longdouble infinity to integer, NumPy is too old:

python -m pip install --upgrade numpy pandas
1.4 Files won't run?
The file name must end in .py (e.g. week3.py), or PyCharm treats it as a text file.
Right-click the file → Run.
2. Which model needs which packages
Packages every tutorial needs
Install name	Import in code	What it does
numpy	import numpy as np	Maths and arrays
pandas	import pandas as pd	Loads and cleans the dataset (tables)
matplotlib	import matplotlib.pyplot as plt	Draws charts (screenshot these for the report)
scikit-learn	import sklearn / from sklearn...	Nearly all the ML models and scores

⚠️ The install name is not always the import name. Watch out for these:

Install with…	…but import as
scikit-learn	sklearn
fuzzy-c-means	fcmeans
beautifulsoup4	bs4
Week 2: Classification and ensembles (predicting a category, e.g. yes/no)

Use when: the dataset has a label column with categories (e.g. churn yes/no, cancer benign/malignant).

Install:

python -m pip install numpy pandas matplotlib scikit-learn

Models used:

DecisionTreeClassifier: a simple flowchart of yes/no questions. Easy to explain to a business.
KNeighborsClassifier (KNN): labels a new point based on its nearest neighbours.
SVC (support vector machine): draws the best boundary line between classes.
RandomForestClassifier: many decision trees voting together. Usually more accurate.
BaggingClassifier, AdaBoostClassifier, VotingClassifier: ways of combining models (ensembles).

How to judge it: accuracy_score (% correct) and f1_score (better when one class is much rarer than the other). Also cross_val_score for a fairer average over several data splits.

Week 3: Clustering (finding groups with no labels)

Use when: there is no label column and you want to find customer segments or similar groups.

Install:

python -m pip install numpy pandas matplotlib seaborn scikit-learn fuzzy-c-means

If fuzzy-c-means downgrades NumPy and breaks things, run:

python -m pip install --upgrade numpy
python -m pip install --no-deps fuzzy-c-means
python -m pip install pydantic tabulate typer tqdm

Models used:

KMeans: splits the data into k groups. You choose k (use the elbow method or silhouette score).
FCM (fuzzy c-means): like KMeans, but each point can partly belong to several groups.
DBSCAN: finds groups based on how dense the data is and flags outliers as noise. You don't choose k.
AgglomerativeClustering: builds groups bottom-up by merging the closest points (hierarchical clustering).

Helpers:

StandardScaler: always scale before clustering, otherwise big-number columns dominate.
PCA: squashes many columns into 2 so you can plot the clusters.
silhouette_score: from -1 to 1, higher means better-separated clusters.
Week 4: Association rules (market basket analysis)

Use when: the data is transactions (what items were bought together), e.g. "people who buy bread also buy butter".

Install:

python -m pip install numpy pandas mlxtend

Models used:

apriori: finds item combinations that appear often.
association_rules: turns those into "if X then Y" rules.

How to judge it:

Support: how often the items appear together.
Confidence: if someone buys X, how likely they are to also buy Y.
Lift: above 1 means X and Y really are linked, not just a coincidence.

Data must be: one row per transaction, one column per item, with values True/False (or 1/0).

Week 6: NLP and sentiment analysis (text data)

Use when: the data is text (reviews, tweets, comments) and you want to classify it, e.g. positive or negative.

Install:

python -m pip install numpy pandas matplotlib scikit-learn nltk beautifulsoup4 wordcloud

Extra one-off downloads (already at the top of the Week 6 file):

python
nltk.download('stopwords')
nltk.download('punkt')
nltk.download('wordnet')

Tools used:

BeautifulSoup (bs4): strips HTML tags out of the text.
re: regular expressions, which remove punctuation and numbers. Built into Python, no install needed.
stopwords: removes filler words like "the", "and", "is".
WordNetLemmatizer: turns words into their base form ("running" becomes "run").
CountVectorizer / TfidfVectorizer: turn words into numbers a model can use. TF-IDF gives less weight to very common words.
WordCloud: a picture of the most common words. Good for the report.

Models used: MultinomialNB (Naive Bayes, fast and good for text), LogisticRegression and SGDClassifier.

How to judge it: accuracy_score, classification_report (precision, recall and F1 for each class) and confusion_matrix.

Not in my tutorials, but could come up: Regression (predicting a number)

Use when: the target is a number (price, sales, salary).

python -m pip install numpy pandas matplotlib scikit-learn
LinearRegression, RandomForestRegressor
Judge with r2_score (closer to 1 is better) and mean_absolute_error (average size of the error).
2b. Model guide (super simple version)

Each model has an everyday example, one line on how it works, then 👍 good and 👎 bad.

Classification: putting things into groups like Yes/No (Week 2 and Week 6)

Decision Tree

🧠 Like a game of 20 Questions: "Is age over 40? Yes → Is income low? No → Answer: will buy."
👍 Easy to explain to anyone. You can draw it.
👎 It can memorise the training data instead of learning it, so it does badly on new data. Fix this with max_depth=3.

KNN (K-Nearest Neighbours)

🧠 "Tell me who your friends are and I'll tell you who you are." It looks at the 5 most similar people and copies what most of them did.
👍 Really simple idea.
👎 Slow with lots of data. You must scale the data first.

SVM (Support Vector Machine)

🧠 Draws the widest possible road between two groups so they're clearly separated.
👍 Very accurate.
👎 Slow on big data, hard to explain, and you must scale the data.

Random Forest

🧠 Ask 100 decision trees and go with the majority vote, like asking the audience on Who Wants to Be a Millionaire.
👍 One of the most accurate. Tells you which columns matter most. No scaling needed.
👎 Harder to explain than one tree. A bit slower.
⭐ Safe choice if you're unsure.

Bagging

🧠 Train the same model lots of times on different random bits of the data, then vote.
👍 More stable and less memorising.
👎 Slower and harder to explain.

AdaBoost

🧠 Like a student re-doing only the questions they got wrong. Each new model focuses on the previous model's mistakes.
👍 Very accurate.
👎 Gets confused by messy data and outliers.

Voting Classifier

🧠 A panel of different judges (tree, KNN, SVM) who all vote.
👍 Usually better than any one judge alone.
👎 Slow and hard to explain.

Naive Bayes (text, Week 6)

🧠 Counts words: "'amazing' and 'love' usually mean a positive review."
👍 Super fast, and great for text.
👎 Doesn't understand word order: "not good" just looks like "not" + "good".

Logistic Regression (Week 6)

🧠 Gives a percentage chance: "This customer has an 80% chance of leaving."
👍 Fast, simple, and gives percentages a business can use.
👎 Only finds simple, straight-line patterns. Should scale the data.

SGD Classifier (Week 6)

🧠 Like logistic regression, but learns bit by bit, so it's quick on huge datasets.
👍 Very fast on big data.
👎 Results can change each run (set random_state=42). Needs scaling.
Clustering: finding groups when there are no answers given (Week 3)

K-Means

🧠 You say "find 3 groups", and it sorts everyone into the 3 groups they're closest to.
👍 Simple and fast. Easy to describe the groups, e.g. "big spenders" and "bargain hunters".
👎 You have to guess how many groups. Only finds round blobs. You must scale the data.

Fuzzy C-Means

🧠 Like K-Means, but people can be in more than one group: "70% bargain hunter, 30% loyal customer."
👍 More realistic when people don't fit neatly in one box.
👎 Slower, and the package is fussy to install.

DBSCAN

🧠 Finds crowds of points packed close together. Anyone standing alone is marked as an outlier (-1).
👍 Works out the number of groups itself. Spots weird data.
👎 Hard to pick the settings (eps, min_samples). You must scale the data.

Agglomerative (Hierarchical)

🧠 Everyone starts alone, then the two closest keep pairing up, like a family tree built backwards.
👍 You can draw a tree picture to help choose the number of groups.
👎 Slow with lots of data.

PCA (a helper, not a model)

🧠 Squashes lots of columns into 2 so you can draw a picture of the clusters.
👍 Great for report screenshots.
👎 The new columns don't mean anything in real life.
Association rules: "people who bought this also bought..." (Week 4)

Apriori

🧠 Looks at shopping baskets: "People who buy bread usually buy butter."
👍 Really easy for shops to use: put items together, make deals, recommend products.
👎 Slow with lots of products. It only shows things happen together, not that one causes the other.
Key words:
Support = how often the items are bought together.
Confidence = how often butter is bought when bread is bought.
Lift above 1 = a real link, not luck.
Regression: predicting a number like price (in case it comes up)

Linear Regression

🧠 Draws a straight line through the data: "Every extra bedroom adds £15k."
👍 Simple and easy to explain.
👎 Real life isn't always a straight line.

Random Forest Regressor

🧠 100 trees each guess a number, and you take the average.
👍 More accurate.
👎 Harder to explain.
Cheat table
Model	In one line	Scale first?	Easy to explain?
Decision Tree	20 Questions	No	✅ Yes
KNN	Copy your neighbours	Yes	✅ Yes
SVM	Widest road between groups	Yes	❌ No
Random Forest	100 trees vote	No	🙂 Sort of
Bagging	Same model, many times, vote	Depends	❌ No
AdaBoost	Learn from mistakes	No	❌ No
Voting	Panel of judges	Depends	❌ No
Naive Bayes	Count the words	No	🙂 Sort of
Logistic Regression	% chance of yes	Yes	✅ Yes
SGD	Fast learner for big data	Yes	🙂 Sort of
K-Means	Sort into k groups	Yes	✅ Yes
Fuzzy C-Means	Can be in several groups	Yes	🙂 Sort of
DBSCAN	Find crowds and loners	Yes	🙂 Sort of
Agglomerative	Keep pairing up	Yes	🙂 Sort of
Apriori	Bought together	No	✅ Yes
Linear Regression	Straight line	Recommended	✅ Yes

💬 Sentence for your report:

"I chose [model] because [good thing]. Its weakness is [bad thing], so I [what you did about it, e.g. scaled the data]."

3. Picking two techniques in the exam
Ask yourself…	Then use…
Is there a column with categories to predict?	Classification (Week 2)
Is there a column with a number to predict?	Regression
No target column, want to find groups?	Clustering (Week 3)
Data is items bought together?	Association rules (Week 4)
Data is mostly text?	NLP + classification (Week 6)

A strong combination: clustering to find customer groups + classification to predict an outcome. Or two classifiers compared (e.g. decision tree vs random forest).

4. How to clean data, step by step

Copy this order every time.

Step 1: Load the data
python
import pandas as pd
df = pd.read_csv("data.csv")      # for Excel: pd.read_excel("data.xlsx")
Step 2: Look at it
python
print(df.head())        # first 5 rows
print(df.shape)         # (rows, columns)
print(df.info())        # column types and missing values
print(df.describe())    # min, max, mean: spot weird values

📸 Screenshot this for your report. It shows you understood the data.

Step 3: Remove duplicate rows
python
print(df.duplicated().sum())   # how many duplicates
df = df.drop_duplicates()
Step 4: Deal with missing values
python
print(df.isnull().sum())       # missing values per column

Pick one approach:

python
df = df.dropna()                                        # delete rows with gaps (only if there are few)
df["Age"] = df["Age"].fillna(df["Age"].median())        # numbers: fill with the median
df["City"] = df["City"].fillna(df["City"].mode()[0])    # text: fill with the most common value

💬 Report tip: say WHY. "Median was used because it isn't affected by outliers."

Step 5: Drop useless columns

ID numbers, names and anything that doesn't help predict the target.

python
df = df.drop(columns=["CustomerID", "Name"])
Step 6: Fix wrong data types and typos
python
df["Price"] = pd.to_numeric(df["Price"], errors="coerce")   # text like "£5" becomes a number (or a gap)
df["Gender"] = df["Gender"].str.strip().str.lower()         # " Male" / "male" become the same value
Step 7: Handle outliers (extreme values)
python
Q1 = df["Income"].quantile(0.25)
Q3 = df["Income"].quantile(0.75)
IQR = Q3 - Q1
df = df[(df["Income"] >= Q1 - 1.5*IQR) & (df["Income"] <= Q3 + 1.5*IQR)]

A boxplot helps you spot them: df.boxplot(column="Income"); plt.show()

Step 8: Turn text categories into numbers

Models only understand numbers.

python
from sklearn.preprocessing import LabelEncoder
df["Churn"] = LabelEncoder().fit_transform(df["Churn"])   # Yes/No becomes 1/0 (good for the TARGET)

df = pd.get_dummies(df, columns=["City"])                  # one 0/1 column per city (good for FEATURES)
Step 9: Split into X (inputs) and y (answer)
python
X = df.drop(columns=["Churn"])   # everything except the target
y = df["Churn"]                  # the target

(Clustering has no y. Just use X.)

Step 10: Train/test split (classification and regression only)
python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y)   # 80% train, 20% test
random_state=42 gives the same split every time.
stratify=y keeps the yes/no balance the same in both halves. Use it for classification only.
Step 11: Scale the numbers

Needed for KNN, SVM, KMeans, DBSCAN and logistic regression. Not needed for decision trees or random forests.

python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)   # learn the scaling from TRAIN only
X_test  = scaler.transform(X_test)        # apply the same scaling to TEST

⚠️ Never fit on the test data. That's "data leakage", and markers look for it.

Step 12: Check class balance (classification)
python
print(y.value_counts())

If one class is much bigger (e.g. 90% "No"), say so in the report and use F1 score rather than accuracy alone.

5. Model template (fill in the blanks)
python
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

model = RandomForestClassifier(random_state=42)   # 1. choose a model
model.fit(X_train, y_train)                       # 2. train it
y_pred = model.predict(X_test)                    # 3. predict on unseen data

print("Accuracy:", accuracy_score(y_test, y_pred))   # 4. evaluate
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))

Clustering version:

python
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

kmeans = KMeans(n_clusters=3, random_state=42)
df["Cluster"] = kmeans.fit_predict(X_scaled)
print("Silhouette:", silhouette_score(X_scaled, df["Cluster"]))
print(df.groupby("Cluster").mean())   # describe each group: great for business insights
6. Report checklist (500–700 words)
[ ] Business problem: what the company wants to know (1–2 sentences)
[ ] Data issues found: missing values, duplicates, outliers, imbalance, plus how you fixed them and why
[ ] Technique 1 and 2: what they are and why they suit THIS data
[ ] Results: accuracy, F1 or silhouette, etc., with screenshots of code and output
[ ] Business recommendation: what the company should actually do
[ ] Limitations: e.g. small dataset, imbalance, what you'd try with more time
