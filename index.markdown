---
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

- There is no evidence for significant variations in the mood in media quotations across weekdays or months
- The mood in the media became significantly more negative in the first phase of the COVID-19 pandemic
- There are statistically significant differences in mood and subjectivity across media outlets. For example, sports and celebrity magazines tend to be more positive and subjective than daily newspapers
- There are no significant differences in mood in quotes from men and women
- On average, quotations from politicians are more negative than the average quotation

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/books.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> Our Data and Methods </h1>
  </div>
  <span class="page__hero-caption">
    Photo credit: 
    <a href="https://unsplash.com">
      <strong>Unsplash</strong>
    </a>
  </span>
</div>

Initially, we want to give a feel about the data and provide some initial analyses and comments on the methods we will use in our work.

## Quotebank

The Quotebank data set provides quotations in the media from more than a decade. In total, 178 million quotations from 2008 to 2020 are attributed with probable speakers and dates. Using machine learning methods, **dlab @ EPFL** has built a framework for recognizing and attributing quotations from media articles, thus building the data set. However, we have only utilized quotations from 2015 to 2020, distributed through the years as in the following diagram:

{% include_relative /_plots/line_dig.html %}

As you can see, there are some significant "holes" in the data set in 2016. This is a finding that could be important for our future investigations. As well as quotations and their respective speakers, Quotebank attributes quotations to the media outlet in which it was first discovered, how many times it has occurred and how the algorithm recognized it as a quote. Finally, the algorithm attributes the quotations with links to the Wikidata entries of its most plausible speaker.

## Wikidata

Since the quotations are attributed with links to Wikidata entries, we can effortlessly utilize information about the speakers. Wikidata is an open database associated with Wikipedia and provides easy-to-use standardized information. We primarily use Wikidata to identify the gender of the speakers, as well as whether they are politicians or not.

## _Sentiments Analyses_ -- What Are They?

Sentiment analyses aim to identify and extract subjective information in text, telling whether a phrase is positive, negative, or neutral. One may input a sentence, such as a quote, into a sentiment analysis tool and get as output a score representing the sentiment. This will be our primary tool in finding trends in the mood of the media.

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

The above quotation has a compound score of $-0.9891$, indicating that it is quite negative, as you probably predicted when reading it.

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

As you might have thought, the art of dedicating compound scores to quotations is not perfect. Our tool, _Vader_, represents each document or text as a bag of words without capturing the information in word proximity or context. Hence, a lot of the contextual information is lost. Moreover, _Vader_ gives additional weight to unique words and phrases, for instance, words written in uppercase or emojis. On the other hand, the algorithm is not likely to discover the underlying context. It could thus mark a quotation positive, even if it considers a grave matter, such as the quote above.

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

Not surprisingly, the distribution is heavily centered around a compound score of $0.0$. As the compound score of _Vader_ is computed through averaging and only gives weight to certain words, this seems reasonable. However, we see that the distribution appears to be heavier towards the positive values. This may indicate that the quotations in the media are more positive than negative, although neutrality sweeps the board.

{% include_relative /_plots/sub_dist.html %}

Over to the subjectivity, a score of $0.0$ is once again the most frequent. This may be more surprising, as quotations and citations often refer to the speaker's opinions. However, it seems like _TextBlob_'s methods overwhelmingly often end up with a score of $0.0$. This is because _TextBlob_ only gives scores to words existent in the _TextBlob_ lexicon. There are many quotes with no words in the lexicon, thereby receiving a score of $0.0$. Initially, one might think that as _TextBlob_ doesn't capture the underlying context, it doesn't extract the subjectivity either.

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/printing_machine.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> Does the Mood in the Media Evolve Throughout Time? </h1>
  </div>
  <span class="page__hero-caption">
    Photo credit: 
    <a href="https://unsplash.com">
      <strong>Unsplash</strong>
    </a>
  </span>
</div>

People talk about their mood swings in everyday talk. We want to investigate if such _everydays myths_ are present in the quotations of the media as well. To concretize our work, we decided to examine whether the mood evolves during the week and across the year's seasons.

But first, we must zoom out. How can we even draw any conclusions from our data set and analyses only by utilizing libraries for sentiment analysis and making visualizations? Luckily, we have a solution.

## Statistical Tests

To tell whether our results are valid, we utilize statistical tests. More precisely, we use classical _t-tests_ and a variant named _Welch's t-test_. In short, these methods test whether the mean of two groups is statistically significantly different. Both kinds of tests will be used for hypothesis testing, where we will examine whether the following null hypothesis is valid:

_The mean of group one equals the mean of group two._

Using the aforementioned methods, we will get a _p-value_. This value tells us how statistically significant our results are. Or, in other words, the p-value tells us how likely it is, given that the null hypothesis is true, to observe results as extreme, or more extreme, than what we have observed.

For our work, we will use a significance level of $1\%$. This implies that if the p-value given by the methods is lower than $1\%$, we will reject the null and conclude that the means of the two groups are different, given the significance level. Thus, we will say that the results are **statistically significant**.

As this data story is not about statistical tricks and treat, we will leave the subject for now. If you would like a deeper explanation of our methods, you could take a look at our [notebook](https://github.com/epfl-ada/ada-2021-project-gutta-boys/blob/main/project_notebook.ipynb). However, to sum up, we will test whether the means of two groups are equal: If our results are too unlikely to happen with equal means, we will conclude that our results are significant. That was the boring stuff. Now, let us zoom in again.

## How Does the Compound Score Evolve?

As we will use the _compound score_ of _Vader_ to measure the mood in the media, we head off by looking at its evolution throughout the data set. We start by finding the average compound score of each date, resulting in the following plot:

{% include_relative /_plots/line_comp.html %}

The vertical axis shows that the daily average compound score mostly is in the $0.15-0.21$ range. However, there are some significant deviations for the compound scores in 2016 and some smaller deviations in other years. The Argus-eyed reader may remember that the data set contains fewer quotations in the corresponding periods, with only some hundreds of quotes a day. Therefore, it is likely that the deviations result from a lower amount of quotations, giving higher variance.

## _The Weekend Effect_

Indeed, as part of everyday talk, people believe that their mood gets better towards the weekend. _The soon weekend-feeling_, _the friday-feeling_ and so on, people love the feeling of more autonomy, better sleep and a desired freedom from the stressful everyday life. Actually, [researchers](https://www.rochester.edu/news/show.php?id=3525) have found the mood to be better at the end of the week. Therefore, we became interested in whether the quotations of the media reflect this as well.

To further understand the topic, we head off by looking at the number of media quotations each weekday:

{% include_relative /_plots/quotesperday.html %}

As expected, more quotations originate from weekdays than from the weekend. The journalists enjoy the weekend as well.

{% include_relative /_plots/boxplot_day.html %}

Investigating the boxplot, we see that the compound scores across the week are very similar. The median scores for the different days are quite similar, but have some variation. However, the mean scores are too similar to conclude on any significant differences in the mood across weekdays.

## _The Season Effect_

Disenchanted by the lack of differences across weekdays, we move towards the next everyday myth. That the weather affects the mood is indeed a belief in society. When the sun is shining, we hurry outside, explore the world, and enjoy life. On the other hand, rainy days call for dark times, slight depression, and frustration. So again, do the media represent the same deviations in the mood? To ease our analyses, we decide to rather search for differences across the months and seasons of the year, assuming that the weather is better during summer.

To head off, we once again look at the distribution of quotations throughout the year:

{% include_relative /_plots/quotespermonth.html %}

There are some differences in the number of quotations per month in our data set. These differences correspond to the previously mentioned periods with few quotes.

{% include_relative /_plots/boxplot_month.html %}

As for the weekday analysis, the month analysis shows minimal differences. Even though the medians vary slightly around $0.12$, the interquartile range, indicating where half of the data is placed, is approximately equal for all twelve months. It is once again not possible to conclude on any differences in the mood of the quotations across months and seasons, maybe implying that the weather does not affect our mood as much as we believed. Actually, further [investigations](https://www.houstonmethodist.org/blog/articles/2021/jul/can-weather-affect-your-mood/) shows that researchers also have found evidence for such a hypothesis to be murky. Unluckily, there is no evidence for our first hypothesis, but during the work, the visualizations turned out to be a blessing in disguise.

## _The COVID-19 Effect_

Hitting of the analyses on how the mood evolves throughout time, we illustrated how the compound score develops through our data set. For simplicity, the latter part of the visualization follows once again:

{% include_relative /_plots/covid_line.html %}

The compound score above drops drastically in March 2020. There are other sections of the plot where the compound drops, but it quickly bounces back. For March 2020, the mood seems to stabilize at a lower level.

As an illuminated reader, you have surely drawn the lines already. A particular pandemic hit the western world in March 2020, resulting in lockdowns, fright, and economic cracks. The mood in the population indeed fell in this period, and the diagram points out that the same yielded in the media. The Quotebank dataset provides a unique opportunity to assess how the mood in the media changed during the first lockdowns, and we decided to dive further into the topic.

To assess the changes in the mood, we will study the compound score in the media in January and February 2020 and call this the pre-COVID score. Then, this score will be compared to the score in March and April 2020, namely the COVID score. Indeed, this is a simplification, but it is adequate for our purpose.

The mean compound score of pre-COVID is $0.19$, versus the mean compound score of COVID at $0.16$. In other words, the compound score was $%18.75$ higher pre-COVID. Thus, the mood in the media was better before lockdowns.

Unsurprisingly, our results provide evidence towards the _The COVID-19 Effect_. However, it is important to keep in mind that the results do not prove causation, only correlation. That is, there could be another factor, outside our analysis, that impacted the mood in media.

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/newspapers.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> How Does the Mood Differ Across Media Outlets? </h1>
  </div>
  <span class="page__hero-caption">
    Photo credit: 
    <a href="https://unsplash.com">
      <strong>Unsplash</strong>
    </a>
  </span>
</div>

We can say that the mood of the quotations in the media doesn't seem to vary too much across time except for when a global pandemic hits. However, as the press eventually decides what reaches the public debate and your mind, exploring how the mood differs across the media could be exciting. Which newspapers are more positive, and which are more negative? If you want to kick-off the day in a good mood, should you read Wall Street Journal or Peoples Magazine?

Initially, it is reasonable to believe that the compound and subjectivity scores of a daily newspaper are different from those of a celebrity magazine. But does the Quotebank data set provide evidence for such a hypothesis? To investigate this, we focus on a subsample of the media outlets represented in Quotebank. The subsample is a selection of the most well-known English-writing newspapers and magazines aiming to represent different categories. As earlier, we use _Vader_ and _TextBlob_ to compute each quote's compound score and subjectivity score, eventually grouped by media outlet and outlet category.

The following plot shows the relative amount of quotations in each of the chosen categories: Newspapers, sports magazines, celebrity magazines, and "others.".

{% include_relative /_plots/pieechart_category.html %}

Starting the analyses, we plot the average compound score against the average subjectivity score for the selected media outlets. The size of the bubbles refers to the corresponding number of quotations attributed to the media outlet:

{% include_relative /_plots/scatterplot_medias.html %}

Clearly, there is a difference! Even though the outlets are situated within a relatively tiny part of the area, there are significant differences in the two scores. Let's consider two extremes: the New York Times and ESPN. The reputable New York Times, a daily newspaper, has an average compound score of $0.085$, while the sports magazine ESPN, providing the latest updates from the American sports life, receives a score of $0.278$ - more than three times as high. On the other hand, the subjectivity scores of the outlets are $0.344$ and $0.405$, respectively. The differences are clear and statistically significant by our methods. Is this an example of a general trend for newspapers and sports magazines?

For further investigations, we average the scores within each category, eventually getting the following distributions:

{% include_relative /_plots/polarity_distribution_categories_same_plot.html %}

The differences are significant. The amount of positive quotations in sports magazines and celebrity magazines clearly surpasses the amount in the daily newspapers, having average compound scores of $0.27$ and $0.17$ versus $0.14$, respectively. This might be reasonable, as daily newspapers should cover all the suffering in society, while sports magazines might focus on victorious athletes.

However, analyses of our data set seem to suggest that if you are influenced by what you read in the media, put away your newspapers and start reading some sports news!

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/people.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> Does The Mood Differ Between The Genders? </h1>
  </div>
  <span class="page__hero-caption">
    Photo credit: 
    <a href="https://unsplash.com">
      <strong>Unsplash</strong>
    </a>
  </span>
</div>

As well as wondering how the mood differs across different media outlets, we asked whether we could find any differences across the genders. Do the quotations in Quotebank reveal any relationship between the genders, for instance that one is more moody than the other?

[comment]: # "Tried to rephrase."

Diving into the data set, we first see a difference in the number of quotations spoken by each gender. Of approximately 16 million attributed quotations, only one-eight are attributed to women.

{% include_relative /_plots/marcus/pie_genders.html %}

Despite the differences in the number of quotations, we could still search after differences in the mood. Well, the data set reveals a glimpse of our hypothesis: Quotations by women have slightly higher compound scores than quotes by men. The difference is statistically significant, but minimal, as can be seen in the plot below. All over, the difference is too small to conclude on any differences. Men and women seem to be just as happy.

{% include_relative /_plots/marcus/line_dist_comp_males_females.html %}

The diagram above shows that the average scores across gender are pretty equal, but the compound score for females varies more than that of men over time. However, this does not prove that women are more moody, but is probably related to the lower number of quotations attributed to women. We also see this from visualizing the normalized distributions of the two subsets as below:

{% include_relative /_plots/marcus/line_compound_males_females_week.html %}

Summing up, we cannot point one gender out as the _moody gender_. However, maybe another group of the population differs from the rest of us?

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/rally.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> Are Politicans More Negative? </h1>
  </div>
  <span class="page__hero-caption">
    Photo credit: 
    <a href="https://unsplash.com">
      <strong>Unsplash</strong>
    </a>
  </span>
</div>

Elected by the people, for the people. Politicians are elected to represent and work for the average Joe and thus have a great responsibility. It is, therefore, reasonable that the politicians receive significant attention from the Fourth Estate. By attributing the quotations in Quotebank to whether it was spoken by a politician or not, we see that about $2\%$ of the quotations belong to politicians.

[comment]: # "Er politikere overrepresent i Quotebank? Må eventuelt se på prosent av befolkningen som er politikere."
[comment]: # "Svar: har det noe å si? Men ja, de vil nok være det av natur"

{% include_relative /_plots/marcus/pie_pol.html %}

As politicians are elected to represent us, do they represent us in the sense of their mood? As of the data, the answer is no. While the average compound score of politicians was $0.156$, the mean compound score of the quotations in Quotebank was $0.191$. As the results are significant, we could say that politicians’ quotes are more negative than the others. The results are also reflected in the plot below.

{% include_relative /_plots/marcus/line_compound_pol_all_week.html %}

We observe that the average compound score for the whole data set is consistently higher than the mean score for politicians. The score for politicians varies more, but that is likely due to low sample sizes.

Furthermore, it is reasonable to believe that politicians base their decisions on factual information, even though this belief might have been weakened in the past years. It is intriguing to hope that Quotebank could give us answers to this through the subjectivity score computed by _TextBlob_. Below, you can see the distribution of subjectivity politicians’ subjectivity scores versus overall subjectivity.

{% include_relative /_plots/marcus/bar_dist_sub_pol_all.html %}
[comment]: # "Decrease bin size"

The subjectivity score for politicians is a bit lower than that of the overall data set, with a score of $0.107$ versus $0.120$. This indicates that politicians are slightly more objective than the average. This is, in itself, quite positive. Politicians should base their choices on factual information, and the data set indicates that they at least communicate this. However, one should remember the examples of subjectivity scores we started with. The computations of subjectivity scores are not a precise science, and simply using _objective_ words could give a better degree of objectivity. Do you remember the last “objective” example of Donald Trump describing “Sleepy Joe”? Maybe not the most precise measure of objectivity...

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/tools.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> What Could We Improve? </h1>
  </div>
  <span class="page__hero-caption">
    Photo credit: 
    <a href="https://unsplash.com">
      <strong>Unsplash</strong>
    </a>
  </span>
</div>

Before concluding that everyone should quit their New York Times subscription, start reading celebrity magazines, and classify politicians as moody, we should take a look on the weaknesses of our analyses.

The first and most apparent objection concerns the data. Quotebank is a vast data set giving unique insights in the media, but is only containing quotations. Quotations makes out only a small subset of the total text published by the media. Therefore, our conclusions on the media are only valid for the **quotations** in the media, and not for the media in general. The quotations do not necessarily reflect the mood of the editorial staff. However, one might believe that articles containing positive quotations are mainly positive, but this is nothing but a hypothesis. To say anything about the total mood in the media, one must do further research on articles, comments and other texts as well.

Secondly, sentiment analysis is not a precise science. As previously mentioned, the methods we have utilized solely consider the sentences as bags of words. The methods do not take the context into account. To increase the robustness of our results, we did also test our hypotheses using _TextBlob's_ polarity score in addition to Vader’s compound score. _TextBlob's_ polarity score works in the same manner as _Vader's_ compund score, by using a lexicon to score individual words. The distributions for TextBlob and Vader are compared in the following plot:

{% include_relative /_plots/comp_pol.html %}

The distributions for the two measures are evidently different. _Vader's_ compound score is more evenly spread out, while the polarity score has few extreme values. Even though the distributions of the scores for _TextBlob_ differs from the one for _Vader_, the test results are similar. This is because they agree on the relative direction, even though the absolute value of the scores differs. This indicates that our results are relatively robust. However, neither _TextBlob_ or _Vader_ consider the content, and a smarter sentiment analysis tool could be utilized to further improve our analyses. For instance, _BERT_, the natural language processing model of Google, takes the context of sentences and words into consideration. _BERT_ has outperformed more straightforward sentiment analysis methods, like _Vader_, for many applications, and could have improved our results' accuracy.

Finally, we would like to point out the importance of not mistaking correlation for causation. For instance, for the COVID-19 effect, even though there is statistically significant evidence for the mood in the quotations being drastically lower when the pandemic hit the western world, we cannot conclude that this is caused by COVID-19. There could be another variable causing the mood and the compound to drop. To further investigate ths, we could utilize instrumental variables to isolate the effect of the pandemic.

<div
  class="page__hero--overlay"
  style="height: 300px; background-image: url('./images/library.jpg'); filter: grayscale(100%);">
  <div class="wrapper">
    <h1 class="page__title" itemprop="headline"> What Have We Learned? </h1>
  </div>
  <span class="page__hero-caption">
    Photo credit: 
    <a href="https://unsplash.com">
      <strong>Unsplash</strong>
    </a>
  </span>
</div>

Throughout our analyses, we have both confirmed and rejected some of our initial ideas about the mood, and how it is reflected in the media. Analyzing the quotations in Quotebank have shown us that people's quotations are more positive than those of politicans, while celebrity magazines are generally more subjective than daily newspapers.

Even though we did not find evidence for some of our _everyday myths_, analyzing Quotebank revealed that COVID-19 affected the mood in the media negatively. Thus, the analyses have offered some results, even though we still don't know whether we love Mondays deep inside, or whether the dark winters lower our mood. If such effect exists, they are not reflected in the quotations we have analyzed.

So next time you hear a heated discussion on the mood in the media, you could say "Listen, I will tell you a thing or two about mood."

