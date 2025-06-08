---
blog-title: Highlights from ML Prague Conference 2023
blog-subtitle: Insights and Takeaways
blog-published: 2023-06-08
blog-tags:
  - Conference
  - NLP
  - LLM
  - ML
  - EN
  - Data Science
---

Last week, I joined machine learning enthusiasts from around the world in Prague for Europe's biggest conference on ML and AI applications, [ML Prague 2023](https://mlprague.com/).

![ML Prague Conference 2023 sign at the entrance](/images/mlprague_sign.jpg)

The event featured a broad lineup of speakers, covering a wide range of topics - from the latest breakthroughs in natural language processing to cutting-edge computer vision applications. In this blog post, I want to write about some of the most interesting talks from the conference and highlight the key takeaways. Without further ado, let's dive in and start with two topics from the area of image processing.

## Image Processing 
### 3D Pose Estimation in Sport 

[Piotr Skalski](https://www.linkedin.com/in/piotr-skalski-36b5b4122/) from Roboflow showed us how he was able to reproduce parts of the Video Assistant Referee (VAR) system shown in the following clip.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/WycjDx6giVE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

He used [YOLOv7](https://github.com/WongKinYiu/yolov7) and two cameras that record from different perspectives to track a skeleton of his body.

When using this technique, there are some key points to consider: 

-  Use **at least** two cameras with the same configuration 
	- For best results, position them above the playing field to reduce player occlusions 
	- For better results, use more than two cameras 
-  Avoid or remove any lens distortion before processing the video recordings.

Take a look at his [GitHub repository](https://github.com/SkalskiP/sport) to see the results.

### Multi-Model Machine Learning based Industrial Vision Tool for Assembly Part Quality Control

[Aimira Baitieva](https://www.linkedin.com/in/aimira-baitieva/?originalSubdomain=cz) from Valeo offered insights into anomaly detection in images using neural networks. 

One interesting idea was to train a model that outputs embeddings for images, which represent their information well enough. You could then create a *"good"* embedding for normal parts (without anomalies) to compare against. If an embedded image has a dissimilarity above a certain threshold when compared with the good embedding, one can mark it as an anomaly.

Their final **multi-model** approach, consisted of three steps: 

1. Create a segmented anomaly map: 
	1. Segment the image to get a segmentation map 
	2. Use an anomaly detector to get an anomaly map 
	3. Combine both maps
2. Extract features from this map 
3. Classify the image based on the features.

![Image at Conference of multi-model approach](/images/mlprague_valeo.jpg)

This turned out to work well and is in use right now.

## Large Language Models 

### LLM-driven Game Characters 

[Marek Rosa](https://www.linkedin.com/in/marekrosa1/?originalSubdomain=cz) from GoodAI shared their progress in creating LLM-driven game characters. 

In their AI Game, characters use a large language model to generate their thoughts, actions and speech. The player can interact with them via a chat interface and with normal in-game actions like exchanging objects. 

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/G7ZAPwji4i0?start=92" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

Every character has a long-term memory in the form of a vector database. The app preprocesses new thoughts and interactions and saves them into the database. When needed, it queries the database based on

- recency 
- importance 
- and relevance.

The current context of the language model forms the short-term memory. This combination of short and long-term memory let's agents learn continually by using it to plan actions based on past observations, experience, its current environment and thoughts. 

Personally, I think that LLMs will redefine the role of NPCs (Non-Player Characters) in video games. Today, the player-controlled character is often the central figure in the game's story while other characters are less important.
In the future, the player will take on a more supporting role and the game does not have a defined protagonist. The story will be open-ended and allows the players **and** NPCs to make choices that affect the outcome and direction of it. 
More games will adopt a sandbox-style approach, like Minecraft, where there is no clear goal or ending of the game. 

Other interesting projects that try to use LLMs to create autonomous agents are 

- [AutoGPT](https://github.com/Significant-Gravitas/Auto-GPT)
- [Voyager Minecraft Agent](https://github.com/MineDojo/Voyager)

## Conclusion 

In conclusion, the ML Prague Conference 2023 was a fantastic opportunity for ML professionals to come together, learn about the latest trends and innovations in the field, and connect with others. I had a great time and am glad that I was able to be there. I hope you enjoyed this short recap of the conference. If you've been there too, let's connect on [LinkedIn](https://www.linkedin.com/in/marcjulian/) and have a chat about **your** favorite talks.

![Prague City with evening sun](/images/mlprague_evening.jpg)