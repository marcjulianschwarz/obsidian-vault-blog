---
blog-title: Hackbay 2023 - A hackathon for everyone
blog-subtitle: Disrupting the DATEV HR Process with AI
blog-published: 2023-06-05
blog-tags:
  - EN
  - Hackathon
  - NLP
  - LLM
  - ML
  - Data Science
---

Last month, I attended the two-day [Hackbay 2023 Hackathon](https://www.hackbay.de/) and, together with [Maximilian Kasper](https://www.linkedin.com/in/maximilian-kasper-693648174/), we worked on DATEV's challenge called *"Disrupting DATEV's HR Process with AI"*.

![Hackathon](/images/hackbay.jpg)

## Our Prototype 

On day one, we started with some brainstorming to generate good ideas that could make the HR process as easy and frictionless as possible for both the applicant and the company. In the end, we decided to concentrate on the part of the process where applicants search for job opportunities and companies try to find fitting candidates for their job offers.

We wanted to create a job-offer platform that uses new advances in Natural Language Processing (NLP) to automatically suggest fitting jobs based on **raw resume text** files. Applicants should be able to upload their resume and instantly see jobs that fit their academic and professional career, their skills, languages, and interests. Also, the applicant should be able to weigh their experiences and interests in a way such that only relevant jobs will be shown.

The platform removes the need for creating a detailed profile and relies only on the applicant's resume. This way, no extra work is necessary.

![Coding at the hackathon](/images/hackbay_mac.jpg)


We decided to use [Streamlit](https://streamlit.io/) as the frontend framework to quickly create a user interface that was fast to iterate on. 

For the recommendation system, we used the following approach:
1. Create an embedding for the resume text 
	1. Use a `BertTokenizer` to encode the text 
	2. Use a `BertModel` to embed the encoded tokens 
2. Create an embedding for every job posting 
	1. Use a `BertTokenizer` to encode the text 
	2. Use a `BertModel` to embed the encoded tokens 
	3. Use an `AnnoyIndex` to store the embedding vectors for easy and fast retrieval via their similarity  ([spotify/annoy](https://github.com/spotify/annoy))
3. Query the index with the resume embedding to get the **k** most similar job postings 
4. Filter and sort similar job postings based on the user's selected weights and hard constraints (e.g. location, language, etc.)

So, in the end, an applicant was able to upload their raw resume to the platform, which would automatically find the most fitting job postings and output them as a sorted list. The applicant could then easily apply to one or more of the listed jobs. 

Of course, this was only a prototype, so there were still a lot of issues that one would have to tackle to actually bring the platform into production.

For example, at the moment, companies and/or applicants could easily *hack* the recommender by writing their job postings and resumes in a way such that they are semantically very similar. This could be achieved by long keyword lists that outweigh all other factors.

## Conclusion 

In summary, we developed a prototype job-offer platform that utilizes NLP to suggest fitting job opportunities based solely on a resume text file. The platform removes the need for an applicant to create a detailed profile, simplifying the hiring process for both employers and applicants. 

Overall, attending the Hackbay 2023 Hackathon was a valuable experience that allowed us to challenge ourselves to develop a ML solution within a short timeframe.

Additionally, you can check out the aftermovie for the 2023 Hackbay Hackathon to get a glimpse of the event's highlights:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/IyNvbw1OFq4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>