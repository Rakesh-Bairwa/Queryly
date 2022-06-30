# GtechX

# Dataset:
Dataset Link: https://www.kaggle.com/datasets/stackoverflow/stacksample

# Seeking data:
Before moving to data cleaning let’s just first see how our data looks like, so for that purpose we going to open the questions.csv and answer.csv and count each rows & columns and also check the datatype of it.

# Data cleaning:
Before building the model we firstly need to clean our our question dataset will going to used for further processing, since its taken from stackoverflow webpage so it have html tags, digits, formulaes, non-ASCII characters & what not. So we going to use regex (or regular expressions to deal with it). We going to only consider ASCII characters, converting upper cases words to lower case, removing html tags, removing http links, making words like haven’t can’t to can not, have not, viz; removing those short hand keywords & later remove all stopwords (stop words are those which aren’t helping us make better models & this words came very frequently inside our text & are basic english words like can, have, why, what, am, his, her etc. This all cleaning will be doing only on questions title.

# Modelling:
1. Using Stackoverflow 10% data from kaggle.
2. Loading data in python (using csv)
Why csv (whyn’t pandas)? A good search engine have high precision & speed with low computational & server cost. So if we use pandas to store this big data we’ll end up making a dataframe → extra space as well extra time to compute. By using basic csv we’re getting way faster speed.
        Pandas used 5+ gb of ram to process & making them in dataframe whereas just using csv
        Process it in just 200mb
3. We’ve multiple solved ques-answer for asked query. In Questions.csv we’ve Id, OwnerUserId, CreationDate, ClosedDate, Score, Title, Body. To find most relevant result we use title & body. But processing body can take lot of computation time let’s just use title only. Similarly in Answers.csv we’ve id, body, parentid,...
4. As we know dealing with numbers are easier for ML models then string so, now we’l take all question title from questions.csv & we want to convert it into vector so broadly speaking we going to import an already implemented models called universal sentence encoder(USE) it will convert our question title into a 512-dimensional vector.
5. Now each question have a question id with it which is used as parent id in answers.csv. So we going to find similar solved questions & for those question we going to take mapped solution.
6. Let’s say we built the models it will take a Question(or sentence) as input & returning a vector. But our objective is to find similarity b/w Qi & Qj {let’s say i = 1 & j = 2} now we gonna compare V1 & V2 inorder to find out similarity b/w Q1 & Q2. And this similarity is not just based on keyWord but on semantic sentencing also. Here we’re assuming similarity(Q1, Q2) ~ similarity(V1, V2). And the return most relevant question-answer pair to user.
7. To find the relevancy of solution we can use similarity functions like cosine similarity, euclid distance, manhattan distance, .. and many more. But this this specific problem we choose to use cosine similarity.
8. Using cosine similarity we will find most relevant matching question to the posted query by user.
9. So how this cosine similarity works.
    a. Consider 3 objects P1, P2, P3 then,
    b. Draw a arrow from origin to P1, P2 & P3 (for simplicity consider 2D plot).
    c. Let’s say we want to find which object (P2 or P3 ) is similar to P1. the,
    d. Find angle b/w (P1, P2) & (P1, P3)
    e. Since 𝛳₁₂ <  𝛳₁₃   →  angle b/w P1 & P2 is smaller than angle b/w P1 & P3.
    f. So we can say that P2 is more similar to P1.
    g. To convert degree to real number we use cosine 𝛳. As we know lesser 𝛳 → more value for cosine, so we can say if (Cos 𝛳₁₂ >  Cos 𝛳₁₃) then P1 is more similar to P2.
    Formulae :
    ![1](https://user-images.githubusercontent.com/65413333/176735482-ce691c47-cb53-4186-ad15-d406a0c88f9d.png)
10. Then sort those solved question according to that score → more score → more relevant words & brief answer.
11. So we need to sort this question based on similarity score so we can use python sort function, but i choose to use heap of size no. of result user want to see. So it’s will take same amount of time but the space complexity will be constant here.
12. Now atlast we will make a dictionary of all those relevant results & store as key value pair where key will be question & value will be vector of answers & store it into a csv. So that we can use this csv for future improvement like making UI.

# Result
![2](https://user-images.githubusercontent.com/65413333/176735581-6e2ae9a0-62ed-473c-a1ba-969a74d09faa.png)
