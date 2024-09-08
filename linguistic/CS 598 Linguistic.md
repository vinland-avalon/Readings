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