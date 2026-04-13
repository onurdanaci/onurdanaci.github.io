# 🧠 Commentaries on Patents

**Introduction**

Recently I dove into the realm of patents. The initial motivation came from an anonymous reviewer in one of my grant applications pointing out that big industrial actors might already have a solution to my project (automated tune-up and calibration of superconducting quantum computers using state-of-the-art optimization, machine learning, and reinforcement learning techniques).

In the rebuttal, I gave what I thought was the obvious answer: these solutions are probably trade secrets — they’re not public — so we also need public answers! Supporting this, even the big public research groups don’t share their “secret sauce” with the world; I’ve seen MSc theses on this topic placed under curfew.

Still, that didn’t stop me from diving deep into the ubiquitous world of patents — they’re everywhere. My first introduction came when I searched a few keywords from my own research on Google Patents, and it was a full-blown rabbit hole. Then came the weekly Google Scholar alerts suggesting patent applications from researchers I know.

Next, I heard about the quantum start-up of my distant PhD cohort, Jonathan Olson (his supervisor, the late Jonathan Dowling, was my second supervisor, mentor, and my PI’s PI). His company, Zapata Computing, faced serious patent hurdles and went bankrupt — at least for a while. Wow. Such a formative experience that Olson literally became a patent attorney afterward. I keep seeing his regular LinkedIn posts about patents in the quantum computing world — like how the U.S. Patent Trial and Appeal Board recently overturned previous rulings on the patentability of quantum algorithms.

Very interesting, since I work in a quantum algorithms group myself and read multiple papers on the topic daily. I had honestly never thought about patent protection for algorithms — they always felt “abstract.” Then Olson would post about how some malicious actors file anonymous objections to competitors’ patents in bad faith just to keep them busy. I’m not surprised about cut-throat competition, but I wasn’t expecting Game of Thrones–level drama and conspiracy.
Then I turned on the TV and saw news about Novo Nordisk’s patent expiration for their weight-loss drug Ozempic — how it could lead to thousands losing their jobs. Economists were even downgrading Denmark’s GDP forecasts from 3% to less than half, 1.4%. I had no idea patents could have such macroeconomic gravity, or how much power these offices wield over entire industries.
Soon after, I read about a new patent granted to Nintendo in the U.S. over a Pokémon game — one so broadly defined that Nintendo could basically claim IP over almost any computer game made today. Naturally, that caused an uproar, made global headlines, and eventually escalated to the point that both the U.S. government and the head of the Patent Office had to intervene. Again, wow. I was genuinely shocked at how much mathematical, surgical, and legal precision patent examination requires.

Even though in academia we also act as scientific referees — and can’t afford to be sloppy — the world of IP and patents feels like a minefield for all political and economic actors. Still, it’s not an exaggeration to say that patents are the crème de la crème of technical documents. The fact that they’re filed in the first place means they’ve been vetted countless times and proven valuable enough to justify spending thousands in filing fees — all while exposing your IP to the public.

So, now I need to read and learn more — at least in my own field. In addition to regularly scanning keywords tied to my research, I wrote a Python script that scrapes the European Patent Office database in the category G06N, bringing me two random patents a week: one in AI and one in Quantum.


Hardware vs. Computer-Implemented Inventions

In both classical AI and quantum, I usually encounter two genres of patents: hardware inventions and the so-called Computer-Implemented Inventions (CII).
The first category — hardware architectures like CPUs, GPUs, FPGAs, superconducting qubits, or ion traps — is relatively straightforward. The second, however, is much more confusing.
When defining CII, I intentionally avoid the words “software patents” or “mathematical inventions” (“inventions with a purely mathematical character”), because those are taboo terms in patent offices worldwide. “Software patents” are a kind of reductio ad absurdum — a contradiction in terms.

By international definition, a method, algorithm, or program that operates solely within the framework of a computer, performing functions only within that same framework, cannot be considered an invention of technical character [*]. And if an invention lacks technical character — meaning it doesn’t solve a technical problem — then it’s not patentable.
The only way an algorithm, method, or computer program can be patented is if it’s used to solve a technical problem in the real world, on real hardware. So even if the method itself is “mathematical” or “software,” as long as it contributes to solving a technical problem, it’s eligible for patent protection [*].

This distinction is contentious and often turns into a full-blown legal shouting match — in writing. For example, a case I recently read gives a good sense of the debate:
In 2019, Robert Bosch filed a patent application (EP3572982) for an improvement to a reinforcement learning method. Their claimed invention introduced a different deep learning architecture and a “stochastic” unit. The EPO rejected the patent, arguing that the “stochastic unit” was unclear and that the invention was purely mathematical. Bosch appealed (T 1952/21), citing earlier patented math-based inventions like the RSA algorithm — essentially saying, “Hey, those are mathematical too, so why were they granted patents?” The appeal was also rejected.
In AI and Quantum, distinguishing what is genuinely new and technical is already hard enough — but figuring out whether something qualifies as a CII or pure math/software can be even harder. I suspect this makes the life of examiners and applicants in these fields double, if not triple, as difficult.

Okay, but what are these patents really? What are they good for? And how do we even begin to read and decipher them?

Learning to Read Patents (and Failing Better)
If you’re a researcher in these fields, you probably start your day scanning arXiv or Scirate, especially during the conference season — NeurIPS, ICLR, ICML for AI; QIP or APS March Meeting for Quantum. You’ve learned to “read diagonally” - quickly gauging what are the main motivation, points, results and methods of a paper. How quickly you can do it really depends on the intended audience/journal/conference, field, your background, and level of technicality of the paper. 
A math-heavy paper full of lemma–proof pairs has a very different density than a physics paper with intuitive sketches and colormapped plots. Likewise, a Phys. Rev. Letters paper compresses insights to the extreme compared to a long, pedagogical Phys. Rev. A paper, and ICLR’s page limits impose their own trade-offs between readability and completeness between a paper’s main text and its appendices.
Modern life also forces us to engage with other dense, precise documents — mortgage contracts, insurance claims, parking ticket appeals, tax forms. We learn to read between the lines, to decode legal-English or so-called legalish.
Patents, though, are more than the sum of these two worlds. Reading one for the first time, you might honestly wonder whether you’re having a stroke because of its bizarre techno-legalish hybrid language.


My only solution so far: consistency. Try reading one and fail. Then try again — and fail better. Move on to another. Rinse and repeat. Maybe large language models can already help with this translation — I don’t know, since I haven’t really used them for it except to clarify certain points.
The goal of this blog section, Commentaries on Patents, is to practice reading and commenting on patents consistently until it feels natural. I see it as a muscle that can be trained through repetition and frustration. Using an LLM to do everything would be like cheating in a gym with robotic arms to bench press — pointless, except maybe to measure progress.
So this is my journey — to learn by reading, failing, and reflecting — and hopefully to involve others along the way.

[*] https://www.researchgate.net/profile/Eugenio-Archontopoulos/publication/230818897_Spot_the_difference_a_computer-implemented_invention_or_a_software_patent/links/0fcfd504f2270a49e8000000/Spot-the-difference-a-computer-implemented-invention-or-a-software-patent.pdf

```{tableofcontents}