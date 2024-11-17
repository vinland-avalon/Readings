# Linguistic Accommodation
## Source
Cristian Danescu-Niculescu-Mizil, Michael Gamon, and Susan Dumais. 2011. Mark my words! linguistic style accommodation in social mediaLinks to an external site.. In Proceedings of the 20th international conference on World wide web  
Fusaroli R, Bahrami B, Olsen K, Roepstorff A, Rees G, Frith C, Tylén K. 2012. Coming to terms: quantifying the benefits of linguistic coordinationLinks to an external site.. Psychology Society.
## Reflection
Both papers focus on linguistic accommodation (convergence, alignment, etc.) but analyze it from different perspectives. "Mark My Words" aims to verify this phenomenon in the real world using a large-scale dataset, while "Coming to Terms" explores its effect on cooperative performance. The former study implements a "large-scale, real-world setting" by analyzing Twitter conversations and proposes a novel probabilistic framework to distinguish accommodation from other effects. The latter study involves in-person experiments, assessing the alignment of linguistic practices, success in cooperation, and their correlations. Together, these two papers provide a foundational but comprehensive view of linguistic accommodation. Additionally, both studies employ quantitative analysis, offering valuable insights.

Some questions may worth discussing (as I try to connect this field with large language models, or LLMs):

Do LLMs exhibit a similar tendency toward accommodation? Given that "Mark My Words" has already established an evaluation method, this could be relatively easy to verify.

Could we develop prompts or fine-tune LLMs to make them more inclined to align with users? Would this improve user experience, or, in other words, enhance cooperative performance as demonstrated in "Coming to Terms"?

# Lexical Choice
## Source
Zhao Wang and Aron Culotta. 2019. When do words matter? understanding the impact of lexical choice on audience perception using individual treatment effect estimationLinks to an external site.. In Proceedings of the Thirty-Third AAAI Conference on Artificial Intelligence  
Chenhao Tan, Lillian Lee, and Bo Pang. 2014. The effect of wording on message propagation: Topic- and author-controlled natural experiments on TwitterLinks to an external site.. In Proceedings of the 52nd Annual Meeting of the Association for Computational Linguistics.
## Reflection
Both papers focus on how the smallest unit of language—words—affect text. "When Do Words Matter" investigates automated methods that estimate how a specific lexical choice (changing one word to another) affects perception and compared to empirical approaches RCTs, this newly proposed ITE estimation can be scaled more easily. The second paper conducted a large-scale experiment on Twitter, where, surprisingly, many pairs of tweets containing the same URL and written by the same user but using different wording were found, and it concluded that certain words do attract more attention.

Some questions may worth discussing:

This might be a dummy question, but while "When Do Words Matter" proposes an automated solution—meaning no manual annotation is needed—can it really be used to determine the effect of word choice? In the experiment, gender is an objective annotation for tweets, but what if I want to understand how changing {word A to word B} affects perceptions of age, status, or race?

The second paper explores how different words affect propagation. However, I'm unsure whether platform-related factors were considered, such as search engine and recommendation system preferences. Could these have influenced the results?

# Interactions with Text
## Source
Mark Bernstein. 2009. On hypertext narrativeLinks to an external site.. In Proceedings of the 20th ACM conference on Hypertext and hypermedia (HT ‘09).  
Craig S. Tashman and W. Keith Edwards. 2011. LiquidText: a flexible, multitouch environment to support active readingLinks to an external site.. In Proceedings of the SIGCHI Conference on Human Factors in Computing Systems (CHI ‘11).
## Reflection
Both papers focus on interactions with text in the context of modern technologies. The first paper identifies the incoherence in hyperlinks for narratives and then describes how a generalized implementation of stretchtext can resolve this issue. The "LiquidText" paper proposes an active reading (AR) system that enables various advanced interactions, enhancing the reading experience, especially on multi-touch devices.

Some questions worth discussing:

1. With larger or even multiple screens available (for PCs and phones, as nearly everyone has both a laptop and a phone), is it possible to use multiple displays to replace in-place stretchtext?

2. According to the demo, it seems that a very large screen is necessary; otherwise, everything becomes inconvenient. However, a bigger screen presents new challenges. First, popular multi-touch devices like iPads are not large enough. Second, a bigger screen is no longer portable, which could significantly reduce people's interest in reading. Imao, I even prefer pure reading, which may at most supports highlighting and annotations.

# Interactive Paper
## Source
Pierre Dragicevic, Yvonne Jansen, Abhraneel Sarma, Matthew Kay, and Fanny Chevalier. 2019. Increasing the Transparency of Research Papers with Explorable Multiverse AnalysesLinks to an external site.. In Proceedings of the 2019 CHI Conference on Human Factors in Computing Systems (CHI ‘19).  
Hohman, Fred & Conlen, Matthew & Heer, Jeffrey & Chau, Polo. 2020. Communicating with Interactive ArticlesLinks to an external site.. Distill.
## Reflection
The first paper proposed the "explorable multiverse analysis report (EMAR)" to provide a better way to present research evaluation results and finally increase transparency through "multiverse analysis." It is actually to allow readers to explore large-scale or specific evaluation results interactively without disrupting their reading experience. The second paper is presented as an interactive document, discussing ideas such as "Connecting People and Data," "Making Systems Playful," "Prompting Self-Reflection," "Personalizing Reading," and "Reducing Cognitive Load." Both papers provide detailed examples, which is good to learn from.  
 
Questions worth discussing:  

1. I really appreciate the first work, but I also expect some tools to enhance it. For example, if I find a set of parameters that lead to an interesting result but have not written them down, will it be easy to retrieve it later?

2. Compared to traditional papers, interactive text does not adhere to strict standards. Could this cause inconvenience in some scenarios, such as reviewing and printing? How can we ensure coherence between the interactive text version and the printed version?

# Creative Interactions
## Source 
Qian Yang, Justin Cranshaw, Saleema Amershi, Shamsi T. Iqbal, and Jaime Teevan. 2019. Sketching NLP: A Case Study of Exploring the Right Things To Design with Language IntelligenceLinks to an external site.. In Proceedings of the 2019 CHI Conference on Human Factors in Computing Systems (CHI ‘19).
Minsuk Chang, Stefania Druga, Alexander J. Fiannaca, Pedro Vergani, Chinmay Kulkarni, Carrie J Cai, and Michael Terry. 2023. The Prompt ArtistsLinks to an external site.. In Proceedings of the 15th Conference on Creativity and Cognition (C&C ‘23).
## Reflection
Both papers focus on the creative interaction between designers and AI models. "Sketching NLP" did research on how developers can sketch NLP-related systems/applications/user experience in a more efficient way. It proposes 1) a "notebook" that functions similarly to traditional wireframes, to abstract language interactions 2) as well as solutions to other problems during different stages of designing. "The Prompt Artist" reveals several phenomena for text-to-image models' output and the artists. According to this work, prompts, prompt templates and even model glitches can all be regarded as "art", which is romantic.

Questions may worth discussing:
1. "Sketching" aims to provide solutions when designing a NLP interaction system, which all make sense. However, Could there be an example of how to apply these solutions or advice?
2. The prompts are model-specific. So when saying "prompt as art", should we also specify the TTI model used?
3. The conlusions of second paper are summarized from interviewers' reponses. Could there be a quantitative way to achieve that?

# NLP evaluation
## Source
Elizabeth Clark, Tal August, Sofia Serrano, Nikita Haduong, Suchin Gururangan, and Noah A. Smith. 2021. All That’s ‘Human’ Is Not Gold: Evaluating Human Evaluation of Generated TextLinks to an external site.. In Proceedings of the 59th Annual Meeting of the Association for Computational Linguistics
Marco Tulio Ribeiro, Tongshuang Wu, Carlos Guestrin, and Sameer Singh. 2020. Beyond Accuracy: Behavioral Testing of NLP Models with CheckListLinks to an external site.. In Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics.
## Reflection
Both papers focus on the evaluation of NLP models. The first paper concludes that people can hardly distinguish machine-generated content, even after simple training, based on experimental results. Thus it reminds researchers to be cautious when using human evaluation as the gold standard or a definitive solution. The second paper, "Beyond Accuracy," introduces CheckList, which resembles "behavior testing" or "black-box testing" and is task-agnostic.  

Some questions that might be worth discussing:  

1. In the first paper, the experiment limits content to 100 words. Given that I still don't believe ChatGPT can write something like "The Gift of the Magi," is it worth investigating how human evaluation accuracy changes with text length?

2. The conclusion of the first paper is somewhat scary—does this imply that the Turing test is now outdated?

3. The checklist can only deal with scenarios that can be enumerated. Is there any way to extend it to open-minded questions?

# augment scientific paper reading
## Source
Andrew Head, Kyle Lo, Dongyeop Kang, Raymond Fok, Sam Skjonsberg, Daniel S. Weld, and Marti A. Hearst. 2021. Augmenting Scientific Papers with Just-in-Time, Position-Sensitive Definitions of Terms and SymbolsLinks to an external site.. In Proceedings of the 2021 CHI Conference on Human Factors in Computing Systems (CHI ‘21).
Kyle Lo, et al. 2023. The semantic reader project: Augmenting scholarly documents through ai-powered interactive reading interfacesLinks to an external site.. CACM.
## Reflection
The first paper proposed a tool named "ScholarPhi" to provide explanation and reference for nonce words (including some terms, symbols and equations) in scientfic paper. The second work is a collection of work (AKA Semantic Reader Project) about how to utilze AI to enhance reading scientific papers, including Discovery, Efficiency, Comprehension, Synthesis and Accessibility and corresponding systems like CiteSee, Scim and also ScholarPhi, which is proposed in the first paper.

Questions may worth discussion:

1. The second work introduced CiteSee as an important part to do discovery. The idea to differentiate citations according to personalization, importance, etc. is appealing. It also reminds me that the citation preview always only shows abstract or summaries but maybe only a small part is related to current paper. Could we extend CiteSee to identify related parts within cited paper?

2. As for training, I didn't see much discussion about generalization among different fields. Will that be a big problem?

3. ScholarPhi will extract the nonce words before reading. To step further, can we have user-defined nonce words for someone new to this field?

# Authors and Audience (Personalization)
## Source
Taewook Kim, Hyomin Han, Eytan Adar, Matthew Kay, and John Joon Young Chung. 2024. Authors’ Values and Attitudes Towards AI-bridged Scalable Personalization of Creative Language ArtsLinks to an external site.. CHI.  
Krzysztof Gajos and Daniel S. Weld. 2008. Improving the Performance of Motor-Impaired Users with Automatically-Generated, Ability-Based InterfacesLinks to an external site.. CHI.
## Reflection
Reply from Bohan Wu
The first paper conducted interviews about how AI-bridged creative language arts affects authors' and audiences' received benefits. Its conclusion is fair enough - although AI has potentials to change everything as a third party role in CLA, people or prosumers are still important. One benefit is that AI can personalize each audience's reading experience and the second paper specifically focuses on how such automatic generation can help motor-impaired readers.

Some questions may be worth discussion:

1. For the first paper, the interviewers may have different experiences with AI. Can we take this factor into consideration?

2. I see someone has proposed a really interesting question - which is the better way to collect preference? Yeah, I think that really matters. Some system may collect info from everywhere and some need manually entered info. I think both make sense in some scenarios. When dealing with specific tasks, for example, to help motor-impaired people, entered info can serve as a more accurate and detailed basis. However, when personalizing for general readers, the factors are more complex. So collecting as much info as possible may be a better choice. Like Tiktok or Facebook, nobody knows how they make such accurate recommendations...


# AI-Mediated 
## Source
Jeffrey T Hancock, Mor Naaman, Karen Levy, AI-Mediated Communication: Definition, Research Agenda, and Ethical ConsiderationsLinks to an external site., Journal of Computer-Mediated Communication.
Maurice Jakesch, Advait Bhat, Daniel Buschek, Lior Zalmanson, and Mor Naaman. 2023. Co-Writing with Opinionated Language Models Affects Users’ ViewsLinks to an external site.. In Proceedings of the 2023 CHI Conference on Human Factors in Computing Systems (CHI ‘23).
## Reflection
"AI-Mediated Communication" summarized almost everything about AI-MC, including how to define it (where's the boundary), how such technologies are introduced, how it affects or might affect society and communications. The concerns raised in this work are very interesting, which are about self-representation, trustworthiness and false messages. The second paper specifically focuses on what if your writing-assistant (a language model) has its idea and tendencies. The conclusion is alarming—the impact is significant, and most people are unaware of it.

Some questions may be worth discussing:  
1. The first work uses examples to support its opinions, including one about Duplex's use of "um’s" and "mm-hmm’s." However, I question whether it really matters if we can tell the difference between a bot and a real person. After all, completing the task, or scheduling that meeting in this context, is what really matters.
2. There may be an intuitive reason for such influence in the second paper's conclusion. The analysis is sentence-level. For a question like "social media's impact", the majority of people do not hold a absolute opinion. Instead, they believe its benefits outweigh the side effects, or vice versa. The auto-completion may just remind them of the opposite opinion which they may be too lazy to include or forget to include without such an assistant. To check whether there's such a factor, there could be analysis of turning point, like "despite all this" in Figure 2.

# AI-persuade
## Reflection
"Working With AI to Persuade" conducted three sets of experiments to figure out the quality of AI-generated vaccine persuasion and people's preference. The conclusion is that AI can generate persuasive messages but those AI-annotated messages are more like to be dislike by audience. The second paper focuses more on misinfo and related questions - what are the characteristics of AI-generated misinfo (linguistic differences and expression patterns) and how detection methods perform on that info, and finally calls for improvements to handle AI-generated misinfo.

Some questions may be worth discussing:  
1. In "Working With AI to Persuade", Table 3 shows some messages generated by GPT-3 and CDC, where the first message pair is totally different. Although the authors pointed out that they may not be the same, I don't think there should be such a significant difference.
2. In the "Semantic Lies", the analysis of linguistic difference between AI-generated and human-generated misinfo shows, the former one is less "in tone or self-focused expressions" and "presented stronger emotions". These two tendencies seem to be contradictory with each other to some extent. Could anyone help clarify the difference in more detail?

# Co-Writing
## Source
Katy Ilonka Gero, Vivian Liu, and Lydia Chilton. 2022. Sparks: Inspiration for Science Writing using Language ModelsLinks to an external site.. In Proceedings of the 2022 ACM Designing Interactive Systems Conference (DIS ‘22)
Mina Lee, Percy Liang, and Qian Yang. 2022. CoAuthor: Designing a Human-AI Collaborative Writing Dataset for Exploring Language Model CapabilitiesLinks to an external site.. In Proceedings of the 2022 CHI Conference on Human Factors in Computing Systems (CHI ‘22).

## Reflection
"Sparks" investigated how to use a large language model to support writers in the creative but constrained task of science writing. Based on Sparks system, they did research on 1) whether Sparks can generate good content and 2) how writers cooperate with Sparks. "CoAuthor" also tried to figure out the language model's generative capabilities. CoAuthor itself is a dataset which covers diverse interaction contexts and support various interpretations of good collaboration.

Some questions may be worth discussing:

1. The meaning of "quality" in "Sparks" paper is interseting - sometimes a randomly-generated text can also be regarded as high-quality. Can there be some further research for this part? For example, in which scenarios user prefer random. I guess they should include "for example...".

2. For the second paper, I'm a little confused about "skip prompts". Given that writers could choose to skip prompts, would this choice affect the balance of the dataset across different writing tasks and prompts?

## Norm Shifts
## Source
Eva Sharma and Munmun De Choudhury. 2018. Mental Health Support and its Relationship to Linguistic Accommodation in Online CommunitiesLinks to an external site.. CHI
Cristian Danescu-Niculescu-Mizil, Robert West, Dan Jurafsky, Jure Leskovec, and Christopher Potts. 2013. No country for old members: user lifecycle and linguistic change in online communitiesLinks to an external site.. In Proceedings of the 22nd international conference on World Wide Web
## Reflection
The first work studied how linguistic accommodation impacts the mental health support the authors of posts received. And the authors found there's always a positive link between them. The second paper proposed a framework to track linguistic norm changes. With this framework, they found that for the evolving norms, users would go through a learning phase, followed by a conservative phase, where users gradually get passed by.

Some questions worth discussing:

1. As discussed in the first paper, less conforming posters sometimes receive less support even if they genuinely require immediate attention. And some patients cannot even express their needs well. If we have a LLM-based application or plugin to polish the posts and make them follow the norms, can this problem get mitigated? 

2. Given the first research is conducted on Reddit, does the platforms' algorithms for recommendation system and search engine influence the results? For example, the more confirming posters may gain more exposure?