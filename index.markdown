---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: single
title: "A Thing or Two About Mood"
header:
  overlay_filter: 0.3
  overlay_image: /images/md-mahdi-8SQ_wsDC0uY-unsplash.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
excerpt: Have you ever wondered whether the mood in the media differ throughout the week and across newspapers? We have.
permalink: /
intro:
  - excerpt: '*"The most important decision you take is to be in a good mood."* -- Voltaire'
---

{% include feature_row id="intro" type="center" %}

## Where We Started

_People's moods differ throughout time_. Sometimes, we are affected by personal matters, social affairs, or the weather. Other times, more prominent aspects might affect our mood; we can be affected by the outcome of political elections, international disputes, or long-lasting pandemics. However, we constantly talk about our mood and its causes.

#### Our Motivation

Provided with the QuoteBank data set, we grew interested in what it could reveal about the truth in the everyday talk about our mood. For example, does the temper become better towards the weekend, or do we all love Mondays deep inside? Is the mood better during the sunny summer and worse when the cold hits us in the winter? Moreover, what can we say about the trends of the mood in the media throughout time?

#### Why Is It Important?

Based on our initial questions, our thoughts wandered into what we could say specifically about the mood in the media. As the media sets the agenda for the public debate, it is intriguing to see how much positivity and negativity reach the readers' minds. Which media outlets provide us with the most positivity, and which are more gloomy? Moreover, can we see any differences across the subsets of speakers in the data set? For example, are politicians' quotes more pessimistic than others? Do women tend to be more optimistic than their peers?

We aim to present our findings on the abovementioned topics in the upcoming data story. Starting with the [QuoteBank](https://dlab.epfl.ch/people/west/pub/Vaucher-Spitz-Catasta-West_WSDM-21.pdf) data set provided by **dlab @ EPFL**, we will utilize information from Wikidata to research our questions by sentiment analyses.

## Key Insights

- There are no significant variations or trends in the mood in media quotations throughout the week days or months.
- The mood in the media became significantly more negative in the first phase of the COVID-19 pandemic.
- The mood and subjectivity across media outlets vary significantly. When subsampling the outlets based on category, clusters appear. Sports newspapers and celebrity magazines tend to be more positive and more subjective than daily newspapers.
- The mood in the quotations said by men and women is very similiar, but women are slightly more positive.
- Quotations spoken by politicans are on average more negative than the average quotation in the data set.

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/books.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> Our Data and Methods </h1>
  </div>
</div>

Initially, we want to give a feel about the data and provide some initial analyses and comments on the methods we will use in our work.

## Quotebank

The Quotebank data set provides quotes in the media from more than a decade. In total, 178 million quotations from 2008 to 2020 are attributed with probable speakers and dates. Using machine learning methods, **dlab @ EPFL** has built a framework for recognizing and attributing quotations from media articles, thus building the data set. However, we have only utilized quotes from 2015 to 2020, distributed through the years as in the following diagram:

{% include_relative /_plots/line_dig.html %}

As you can see, there are some significant "holes" in the data set in 2016. This is a finding that could be important for our future investigations. As well as quotations and their respective speakers, Quotebank attributes quotations to the media outlet in which it was first discovered, how many times it has occurred and how the algorithm recognized it as a quote. Finally, the algorithm attributes the quotations with links to the Wikidata entries of its most plausible speaker.

## Wikidata

Since the quotations are attributed with links to Wikidata entries, we can effortlessly utilize information about the speakers. Wikidata is an open database associated with Wikipedia and provides easy-to-use standardized information. We primarily use Wikidata to identify the gender of the speakers, as well as whether they are politicians or not.

## _Sentiments Analyses_ -- What Are They?

Sentiment analyses aim to identify and extract subjective information in text, telling whether a phrase is positive, negative, or neutral. Using libraries, one could input a sentence, such as a quote from the media, and get a score. This will be our primary tool in finding trends in the mood of the media.

[comment]: # "Avsnittet over bør skrives om"

<p style="text-align: center">
<img src="./images/sentiment_tweets.gif" style="position: relative; ">
<figcaption style="position: relative; text-align: center">Source: Hackernoon</figcaption>
</p>

#### _Compound Scores_ -- Huh?

In our analyses, we utilize the _Vader Lexicon_ in the _NLTK_ library to generate _compound scores_. The compound score measures the total sentiment by summing the individual words' sentiment and normalizing the result to a value between $-1$ and $1$. A score of $-1$ indicates the most negative compound, while a score of $+1$ indicates the most positive.

<div>
    <blockquote>
        Fear itself is always more dangerous than the thing you fear. The fear of death is worse than dying. Fear takes you hostage and kills your resistance. Nowhere is fear more fatal than in prison.
        <cite> Mehmet Altan </cite>
     </blockquote>
</div>

The above quotation has a compound score of $-0.9891$, indicating that it is quite negative, as you probably could predict when reading it.

<div>
    <blockquote>
        I was glad to see him, and I'd like to think he was glad to see me.
        <cite> L. Kennedy </cite>
     </blockquote>
</div>

On the other hand, the quotation above receives a compound score of $0.8176$. This is reasonable as it contains light, positive words. However, while these quotations show the extremes on the compound scale, the majority of quotations lie in the middle. For instance, the following quote receives a score of $0.00$, and is therefore considered neutral:

<div>
    <blockquote>
        There's a fact of life. The Assad regime has stabilized the situation in much of Syria.
        <cite> Eran Lerman </cite>
     </blockquote>
</div>

As you might have thought, the art of dedicating sentiment scores to quotations is not perfect. Our tool, _Vader_, represents each document or text as a bag of words without capturing the information in word proximity or context. Hence, a lot of the contextual information is lost. Moreover, _Vader_ gives additional weight to unique words and phrases, for instance, words written in uppercase or emojis. The methods are based on actual human research and are therefore not considered a machine learning method. On the other hand, the algorithm is likely not to discover the underlying context. It could thus mark a quotation positive, even if it considers a grave matter, such as in the quote above.

[comment]: # Fjerne setningen om ML, evt. si litt om human research uten å nevne ML.

#### _Subjectivity Scores_ -- Uhm?

To make our analyses richer, we will also utilize a measure of _subjectivity_. Here, we use the _TextBlob_ library to compute the subjectivity scores of the quotes. The subjectivity score indicates whether a quote's content is subjective, objective, or somewhere in-between. For example, a subjectivity score of $1.00$ indicates a subjective quote, while a score of $0.00$ indicates objectivity. In other words, the subjectivity scores indicate the amount of personal opinions and factual information contained in a text, phrase, or quote.

<div>
  <blockquote>
    Think of that. That crazy Nancy. She is crazy. And shifty Schiff. How about this guy? He makes up my conversation, which was perfect. He makes up my conversation. He sees what I said. It doesn't play well because it was perfect.
    <cite> Donald Trump </cite>
  </blockquote>
</div>

Let's consider the subjectivity score of the quote above. As the speaker of the quote is Donald Trump, you might not be surprised that it receives a subjectivity score of $1.00$. Actually, a large amount of the quotes in Quotebank receive high subjectivity scores simply because they are communicating the speaker's opinions. On the other hand, the following quotation receives a subjectivity score of $0.00$. This is reasonable as it represents an objective fact:

<div>
  <blockquote>
    Boris Johnson got 43 percent of the vote.
    <cite> David Lammy </cite>
  </blockquote>
</div>

As you might have guessed, computing subjectivity scores is not a precise science either. As _Vader_, _TextBlob_ consider texts as bags of words, meaning that it doesn't capture the entire context. Thus, _TextBlob_ gives each word a subjectivity score and averages over the phrase. In addition, the tool looks at _intensity_, determining whether a word affects the next, e.g., adverbs like _very_. However, as the library doesn't capture the entire context and averages the subjectivity scores of words over a phrase, we might discover weird results. For instance, it is probably not reasonable, at least for most of us, that the following quote receives a subjectivity score of $0.00$:

<div>
  <blockquote>
    He's fallen asleep. He has no idea what the hell he's doing or saying.
    <cite> Donald Trump about Joe Biden </cite>
  </blockquote>
</div>

## The Initial Investigations

So, after looking at some examples of quotes and their respective compound and subjectivity scores, you might wonder how these scores are distributed within Quotebank. Firstly, we take a look at how the compound scores are distributed:

{% include_relative /_plots/comp_dist.html %}

Not surprisingly, the distribution is heavily centered around a compound score of $0.0$. As the compound score of _Vader_ is computed through averaging and only gives weight to certain words, this seems reasonable. However, we see that the distribution appears to be heavier on the positive values. This may indicate that the quotations in the media are more positive than negative, although neutrality sweeps the board.

{% include_relative /_plots/sub_dist.html %}

Over to the subjectivity, a score of $0.0$ is once again the most frequent. This may be more surprising, as quotations and citations often refer to the speaker's opinions. However, it seems like _TextBlob_'s methods overwhelmingly often end up with a score of $0.0$. This is because _TextBlob_ only gives scores to words existent in the lexicon. There are many quotes with no words in the lexicon, thereby receiving a score of $0.0$. Initially, one might think that as _TextBlob_ doesn't capture the underlying context, it doesn't extract the subjectivity either. Finally, we note that the remaining distribution is centered around $0.5$ but has a clear upswing towards $1.0$.

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/printing_machine.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> Does the Mood in the Media Evolve Throughout Time? </h1>
  </div>
</div>

People talk about their mood swings in everyday talk. We want to investigate if such _everydays myths_ are present in the quotations of the media as well. To concretize our work, we decided to examine whether the mood evolves during the week and across the year's seasons.

But first, we have to zoom out. How can we even draw any conclusions from our data set and analyses only by utilizing libraries for sentiment analysis and making visualizations? Luckily, we have a solution.

## Statistical Tests

To tell whether our results are valid, we utilize statistical tests. More precisely, we use classical _t-tests_ and a variant named _Welch's t-test_. In short, these methods test whether the mean of two groups is statistically significantly different. Both kinds of tests will be used for hypothesis testing, where we will examine whether the following null hypothesis is valid:

_The mean of group one equals the mean of group two._

Using the aforementioned methods, we will get a _p-value_. This value tells us how statistically significant our results are. Or, in other words, the p-value tells us how likely it is, given that the null hypothesis is true, to observe results as extreme, or more extreme, than what we have observed.

For our work, we will use a significance level of $1\%$. This implies that if the p-value given by the methods is lower than $1\%$, we will reject the null and conclude that the means of the two groups are different, given the significance level. Thus, we will say that the results are **statistically significant**.

As this data story is not about statistical tricks and treat, we will leave the subject for now. If you would like a deeper explanation of our methods, you could take a look at our [notebook](https://github.com/epfl-ada/ada-2021-project-gutta-boys/blob/main/final_version.ipynb). However, to sum up, we will test whether the means of two groups are equal: If our results are too unlikely to happen with equal means, we will conclude that our results are significant. That was the boring stuff. Now, let us zoom in again!

## How Does the Compound Score Evolve?

As we will use the _compound score_ of _Vader_ to measure the mood in the media, we head off by looking at its evolution throughout the data set. We start by finding the average compound score of each date, resulting in the following plot:

{% include_relative /_plots/line_comp.html %}

The vertical axis shows that the compound scores are primarily around $0.12$ with some slight variances. However, there are some significant deviations for the compound scores in 2016. The Argus-eyed reader may remember that the data set contains fewer quotations from the same period as the deviations, with only some hundreds of quotes a day. Therefore, it is likely that the deviations result from a lower amount of quotations, giving higher variance.

## _The Weekend Effect_

Indeed, as part of everyday talk, people believe that their mood gets better on the weekend and towards the weekend. _The soon weekend-feeling_, _the friday-feeling_ and so on, people love the feeling of more autonomy, better sleep and a desired freedom from the stressful everyday life. Actually, [researchers](https://www.rochester.edu/news/show.php?id=3525) have found the mood to be better at the end of the week. Therefore, we became interested in whether the quotations of the media reflect this as well.

To further understand the topic, we head off by looking at the number of media quotations each weekday:

{% include_relative /_plots/quotesperday.html %}

As expected, more quotations originate from weekdays than from the weekend. The journalists enjoy the weekend as well.

{% include_relative /_plots/boxplot_day.html %}

Investigating the boxplot, we see that the compound scores across the week are very similar. The mean scores for the different days are approximately equal, once again varying around $0.2$. Actually, the median compound is highest for Thursdays, Mondays, and Sundays. However, the results are too similar to conclude any significant differences in the mood across weekdays.

[comment]: # "How does the mean vary from the median in the above boxplot?"

## _The Season Effect_

Disenchanted by the lack of differences across weekdays, we move towards the next everyday myth. That the weather affects the mood is indeed a belief in society. When the sun is shining, we hurry outside, explore the world, and enjoy life. On the other hand, rainy days call for dark times, slight depression, and frustration. So again, do the media represent the same deviations in the mood? To ease our analyses, we decide to rather search for differences across the months and seasons of the year, assuming that the weather is better during summer.

To head off, we once again look at the distribution of quotations throughout the year:

{% include_relative /_plots/quotespermonth.html %}

There are some differences in the number of quotations per month in our data set. These differences correspond to the previously mentioned periods with few quotes.

{% include_relative /_plots/boxplot_month.html %}

As for the weekday analysis, the month analysis shows minimal differences. Even though the medians vary slightly around $0.12$, the interquartile range, indicating where half of the data is placed, is approximately equal for all twelve months. It is once again not possible to conclude on any differences in the mood of the quotations across months and seasons, maybe implying that the weather does not affect our mood as much as we believed. Actually, further [investigations](https://www.houstonmethodist.org/blog/articles/2021/jul/can-weather-affect-your-mood/) shows that researchers also have found evidence for such a hypothesis to be murky. Unluckily, there is no evidence for our first hypothesis, but during the work, the visualizations turned out to be a blessing in disguise.

##### _The COVID-19 Effect_

Hitting of the analyses on how the mood evolves throughout time, we illustrated how the compound score develops through our data set. For simplicity, the visualization follows once again:

{% include_relative /_plots/covid_line.html %}

The compound score above drops drastically in March 2020. There are other sections of the plot where the compound drops drastically, but this is the only section with no significant reduction in quotations. Moreover, the mood stays down for some time, actually until the end of the data set.

As an illuminated reader, you have surely drawn the lines already. A particular pandemic hit the western world in March 2020, resulting in lockdowns, fright, and economic cracks. The mood in the population indeed fell in this period, and the diagram points out that the same yielded in the media. The Quotebank dataset provides a unique opportunity to assess how the mood in the media changed during the first lockdowns, and we decided to dive further into the topic.

To assess the changes in the mood, we will study the compound score in the media in January and February 2020 and call this the pre-COVID score. Then, this score will be compared to the score in March and April 2020, namely the COVID score. Indeed, this is a simplification, but it is adequate for our purpose.

The mean compound score of pre-COVID is $0.19$, versus the mean compound score of COVID at $0.16$. In other words, the compound score was $%18.75$ higher pre-COVID. Thus, the mood in the media was better before lockdowns.

To conclude whether the observed difference is significant, we utilize the previously mentioned _t-test_, resulting in a p-value of $0.0$, signifying that the result is very significant. We could therefore conclude that the COVID-19 pandemic affected the mood in the media and that it got worse.

[comment]: # "Could drop parts of the last paragraph."

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/newspapers.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> How Does the Mood Differ Across Media Outlets? </h1>
  </div>
</div>

We can say that the mood of the quotations in the media doesn't vary too much across, unless a global pandemic hits us. However, as the media eventually decides what reaches the public debate and your mind, it could be interesting to explore how the mood differs across the media. Which newspapers are more positive, and which are more negative? If you want to kick-off the day with a good mood, should you read Wall Street Journal or Peoples Magazine?

Initially, it is reasonable to believe that the compound and subjectivity score of daily newspaper is different from that of a celebrity magazine. But does the Quotebank data set provide evidence for such a hypothesis? To investigate this, we choose to focus on a subsample of the media outlets represented in Quotebank. The subsample is a selection of the most well-known English-writing newspapers and magazines aiming to represent different categories. As earlier, we use _Vader_ and _TextBlob_ to compute the compound score and subjectivity score of each quotes, eventually grouped by media outlet and outlet category.

The plot below display the amount of quotes from the different media outlets


{% include_relative /_plots/pieechart_category.html %}

{% include_relative /_plots/treemap_media.html %}

Starting the analyses, we plot the average compound score against the average subjectivity score for the selected media outlets:

{% include_relative /_plots/scatterplot_medias.html %}

Clearly, there is a difference! Even though all the outlets are situated within a relative tiny part of the area, there seems significant variance in the two sores. Let's consider two of the extremes: New York Times and ESPN. The reputable New York Times, a daily newspaper, has an average compound score of $0.085$, while the sports magazine ESPN, providing the latest updates from the american sportslife, receives a score of $0.278$ - nearly three times as high. On the other hand, the subjectivity scores of the outlets are $0.344$ and $0.405$ respectively. The differences are clear, and statistically significant by our methods. Is this an example of a general trend for newspapers and sports magazines?

For further investigations, we assign each media outlet to one of four different subcategories: Newspapers, sports newspapers, celebrity magazines and "others". Averaging the scores within each subcategory, we get the following distributions:

{% include_relative /_plots/polarity_distribution_categories_same_plot.html %}

[comment]: # "Need to have x-axis!"

The differences are significant. The amount of positive quotations in sports newspapers and celebrity magazines clearly surpasses the amount in the daily newspapers, having average compound scores of $0.27$ and $0.17$ versus $0.14$ respectively. This might be reasonable, as daily newspaper should cover all the suffering in the society, while sports newspapers might focus on victorious athletes.

However, it seems safe to conclude that if you are getting influenced by what you read in the media: Put away your newspapers and start reading some sports news!

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/people.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> Does The Mood Differ Between The Genders? </h1>
  </div>
</div>

As well as wondering on how the mood differs across the different media outlets, we wondered whether we could find differences across the genders. Do the quotations in Quotebank reveal what we all know, that men are more moody than women?

Diving in to the data set, we first see a difference in the amount of quotations spoken by each gender. Of approximately 16 million quotations that could be attributed to spoken by either men or women, only an eight of them are attributed to women.

{% include_relative /_plots/marcus/pie_genders.html %}

Despite the differences in the number of quotations, we search after differences in the mood. Well, the data set reveals a glimt of our hypothesis: Quotations by women have a slightly higher compound scores than those spoken by men. The difference is significant, but nevertheless very small: The average sentiment score for women is $0.196$ and $0.194$ for men. All over, we might say the women represented in our data set is a tiny, tiny bit more positive than their peers.

{% include_relative /_plots/marcus/line_compound_males_females_week.html %}

As of the diagram above, it is not easy to see that the average compound score. The diagram also shows that the compound score for females vary more than that of men. However, this does not point towards women being more moody, but is probably related to the lower amount of quotations attributed to women. Below, we also see the compound score distributions of the gender plotted on each other:

{% include_relative /_plots/marcus/line_dist_comp_males_females.html %}

Summing up, we cannot point one of the gender out as the _moody gender_. However, maybe another group of the population differs from the rest of us?

[comment]: # "Both polarity and subjectivity showed the same amount of difference, meaning little. We guess this is good news, as we wouldn't want to hold it against one gender to be more of one thing or another in the public discussion."

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/rally.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> Are Politicans More Negative? </h1>
  </div>
</div>

Elected by the people, for the people. Politicans are elected to represent and work for average Joe, and thus have a great responsibility. Therefore, it is reasonable that the politicans receive great attention by the Fourth Estate. After attributiong the quotations in Quotebank to whether it was spoken by a politican or not, we see that about $2\%$ of the quotations belong to politicans.

[comment]: # "Er politikere overrepresent i Quotebank? Må eventuelt se på prosent av befolkningen som er politikere."

{% include_relative /_plots/marcus/pie_pol.html %}

As politicans are elected to represent us, do they represent us in the sense of their mood? As of the data, the answer is no. While the average compound score of politicans was $0.156$, the average compound score of the quotations in Quotebank was $0.191$. As the results are significant, we could actually say that the quotations spoken by politicans are more negative than the others.

Stating that politicans are more negative, we have to remember that the quotations are from the six last years. The polarity in the society and the public debate has evolved dramatically through the same period. The politians have apparently been in the front line of this development. Therefore, it could be interesting to see whether this is reflected in Quotebank. Firstly, one could look at how the compound scores of the politicans develop throughout the period.

{% include_relative /_plots/marcus/line_compound_pol_all_week.html %}

As earlier, the overall compound score of the quotations are more or less stable throughout the period. On the other hand, we might

Furthermore, it is reasonable to think that politicans base their decisions on factual information, even though this belief might have been weakened the past years. It is intriguing to hope that Quotebank could give us answers to this, and the subjectivity score computed by _TextBlob_ might reflect this. Below, you could see how the subjectivity scores of politicans are distributed compared to the overall subjectivity.

{% include_relative /_plots/marcus/bar_dist_sub_pol_all.html %}

Maybe a bit surprisingly, the subjectivity score for politicans are a bit lower than that of the overall data set, with a score of $0.107$ versus $0.120$. This indicates that politicans are more objective than the average. This is, in it self, quite positive to think about. Politicans should base their choices on factual information, and the data set indicates that the at least communicate this. However, one should remember the examples of subjecitivity scores we started off with: The computations of subjectivity scores are not a precise science, and simply using _objective_ words could give a better degree of objectivity. Do you remember the last, "objective" example of Donald Trump describind "Sleepy Joe"? Maybe not the most precise measure of objectivity...

## What Could We Improve?

Before concluding that everyone should quit their NYTimes subscription, start reading beauty magazines, and classify politicians as moody, we should evaluate the weaknesses of our analysis.

The first and most apparent objection concerns the data. Quotebank is a vast dataset that gives unique insights into the media but only contains quotes. Quotations are only a small subset of the total text published in the press. Our conclusion about the mood in the media is thus only valid for quotations, not for mood in general. We believe it is plausible that articles containing positive quotes are also positive, but this is just our hypothesis. To determine the total mood in the media, further research must be done on other subsets of the media, not only quotations.

Secondly, as previously mentioned, sentiment analysis is not an exact science. The methods we used in this analysis solely consider the sentences as a bag of words. They are not taking into account the context of the words. To increase the robustness of our results, we did also test our hypothesis with TextBlob’s polarity score in addition to Vader’s compound score. The TextBlob polarity score works in the same way as Vader by using a lexicon to score individual words. The distributions for TextBlob and Vader can be seen below:

{% include_relative /_plots/comp_pol.html %}

The distributions for the two measures are evidently different. The compound score is more evenly spread out, while the polarity score has few extreme values. Even though the distributions of the scores for TextBlob differs from the one for Vader, the test results are similar. This is caused by even though the absolute value of the scores differs, they mostly agree on the relative direction. This table shows that our results are relatively robust. However, we have not considered context, and a “smarter” sentiment analysis tool could be utilized to further improve our analysis. For example, BERT, the natural language processing model from Google, also considers the context of sentences. The BERT method has outperformed more straightforward sentiment analysis methods, like Vader, for many applications and could improve our results’ accuracy.

Finally, it is important to not mistake correlation for causation. For example, for the Covid effect, even though there is statistically significant evidence for the mood being drastically lower when Covid occurred, we cannot conclude that this is caused by Covid. There could be another variable that actually caused the mood to drop. To further investigate this, we could use instrumental variables to isolate the effect from Covid.

## What Have We Learnt?

So, what have we learnt about the mood in the media?

Through the analysis we have both confirmed and rejected some of our everyday hypothesis about the mood in the media. Normal people are happier than politicians, celebrity magazines are generally more subjective than newspapers and the late pandemic indeed affected the media mood. However, if there exist something like a Friday effect making us happy towards the weekend, it is not reflected in the quotes we have worked with. Admittedly, women are happier than men, but unless you want to go into a detailed p-value discussion with your spouse, we recommend you not address the topic. The difference is after all very small.

[comment]: # "Vi har ikke funnet ut at kvinner er gladere enn menn. Husk å fiks det."

So next time you hear a heated discussion about the mood in the media. Say listen, I will tell you a thing or two about mood...
