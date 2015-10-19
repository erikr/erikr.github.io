---
title: Tips for technical interviews
layout: post
tags: academia, engineering, research
---

I applied for the Hertz Foundation Fellowship, and was invited to the first round of interviews.

Questions, plus answers for technical topics:

+ Q: Tell me how your journey started, back when you chose a college, then decided to pursue MD/PhD training.
+ Q: Why did you switch your interests from biomaterials to software?
+ A: Software is eating the world (shoutout to A16Z).
+ Q: Where do you see yourself after finishing school, then after that?
+ A: Residency, then faculty with joint appointments in medicine and engineering. I want to balance patient care, research, and teaching in a synergistic fashion. This sounds cliched, but interestingly it is also one of the rarest outcomes for any academic doctor. Everyone says they want to do it because they think the admissions committees want to hear it. They may be right. Lots of us say this because we truly want to do it. But the actual number of MDs and MD/PhDs who successfully attain synergy between three very demanding and sometimes conflicting endeavors is small - based on my anecdotal experiences with faculty over the years.
+ Q: Tell me about a situation in which you exercised creativity?
+ A: I talked about my senior design project at UCLA where we attempted to reduce cell death in polymer scaffolds by seeding cells in a gradient to better control oxygen consumption and diffusion. Our team was limited to 1 or 2-dimensional modeling in COMSOL because of mathematical complexity. To match computational and experimental paradigms, we fabricated polymer discs that fit exactly in the wells of a 48-well plate. Like stacking pancakes of different chocolate chip densities, we stacked discs with different cell densities to crudely control cell distribution.
+ Q: Another thing Hertz looks for is technical skill. What are some areas we can talk about?
+ A: Chemical engineering, transport, biology / physiology, statistics.
+ Q: We play a game where I have 1000 coins and you have 1000 coins. We take turns flipping one coin at a time. If it lands heads, I get the coin (or keep it if it was my flip). If it lands tails, you get it. What does the sample space of game outcomes look like? How would you determine the length of the game?
+ A: I would write some very simple code and run a Monte Carlo simulation, then plot a histogram of outcomes against simulation number. I would expect to see the distribution of game outcome follow a normal distribution if sufficient samples were taken. At this point I cited the central limit theorem.
+ Q: What is an analogous process to this coin-flipping game? What would both the game and the analogous process look like at infinite time?
+ A: I did not immediately answer correctly, but we ended up discussing diffusion.
+ Q: What is the relationship between length scale and diffusion, and why?
+ A: Tau = (lambda)^2 / diffusivity, and I am not sure; in 3D diffusion I would expect the sample space to have 3 dimensions.
+ Q: Switching gears - how would you construct a biological circuit that can perform logical operations?
+ A: I would introduce two receptors on the surface of a cell that had specific binding to two different ligands; say, calcium versus a neurotransmitter.
+ Q: In real life what do the kinetics of ligands binding to a receptor look like, and why?
+ A: Sometimes the response looks like sigmoidal curve, because of allosteric effects, e.g. hemoglobin. Activity reaches an asymptote. There is also lots of noise in biological systems.
+ Q: How would you make a sigmoidal curve sharper?
+ A: First, I thought to increase the binding affinity of the ligand for the receptor, but that would probably just increase binding at all concentrations. Next, I thought about how hemoglobin works; you could make the receptor or enzyme decrease activity at higher concentrations of ligand. Another way is to add a 3rd party allosteric modulator, so I used 2,3-BPG acting on hemoglobin as an example.
+ Q: Let us suppose you construct a surface receptor system that lets you signal 1 or 0 using two separate ligand inputs. How would you build an 'AND' and an 'OR' gate?
+ A: Use a downstream signaling integrator, or traffic the signal to a different part of the cell, a la trafficking motifs. I also talked about a very superficial neural network approach.
+ Q: Describe an organ system from a clinical perspective, then contrast that to an engineering or science view.
+ A: The brain; neurologists think of disease in terms of clinical symptoms or vascular localization. Neuroscientists think about a more microscopic scale, drawing on graph / information theory, electrophysiology, etc. Also, the heart; physiologists clasically think of CV systems as a network of resistors, capacitors, and pumps (I cited Guyton). Cardiologists integrate a lot of physiology (clots to the brain from afib, or fluid overload hitting the lungs, etc.)... actually, cardiology was a bad example because that is a fairly quantitative medical specialty. But I summarized by saying the major difference between the two views was a quantitative approach, or for a bench scientist, length scale versus clinical integration.
+ Q: You mentioned in your application you enjoy cooking. Give me an example of chemical engineering or chemistry in cooking.
+ A: I talked about heat transport in making the perfect filet mignon. Pink in the middle, but seared with a beautiful crust on the outside. You have to use high, intense heat in the beginning to lock in the color and flavor (Maillard reaction), then reduce heat to gently cook the center of the meat further from the heat source. If you want more even diffusion, you can sear on the stovetop and transfer to a convection oven.
+ Q: Outside of your research, what are the three big-picture problems you want to work on?
+ A: 1) Instill rigorous management practices into academia, 2) Apply behavioral economics and psychology so society chooses truly impactful problems to work on instead of problems people think are important, and 3) Use technology in a broad sense (mobile, cloud, analytics) to radically transform training and education.

I asked my interviewer two questions:

+ Q: Why do you contribute to Hertz? What did you get out of it?
+ Q: What do you strongly believe in that most people disagree with you about?
+ A: Global health requires technology innovation, but also cultural and policy approaches. People tend to lie in one camp or the other and criticize work outside of their view. I disagree with both extremists and believe we need a combined approach to improve healthcare in underresourced environments.
