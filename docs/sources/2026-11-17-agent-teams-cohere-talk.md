# Talk: How to get frontier coding results at 70% lower cost - Nouha Dziri, Cohere

Taken on November 17, 2026. A 31-minute conference talk by Nouha Dziri, a senior researcher at Cohere, about her team's paper on teams of coding agents: a lead agent with worker agents, which stage to staff, which model in which role, team size and messages between agents. Video: https://www.youtube.com/watch?v=maitMe9HCvw. The text below is the YouTube auto-generated transcript, unchanged. It is one source for the agent factory idea, which has no meta repo yet.

0:00 Hi everyone. Uh my name is Noa. I'm a senior researcher at Coher and associate industry member at MLA and an adjunct PI
0:08 at the Institute Tubing. I'm so excited to tell you today about how to optimize your agent team and get frontier coding
0:16 at 70% lower the cost. Um this is a work that I did uh at Coher. All right. So traditionally there are two ways to make an agent stronger at inference time.
0:27 First, you can let it think longer, meaning you scale compute sequentially with longer chain of reasoning. And second, you can sample more. You run many independent attempts in parallel.
0:37 And then you aggregate them. But as agent take on longer and longer task, they can navigate large repo. They can
0:44 edit the code. [snorts] They can debug their own work over hours. So what can happen here is that the context window
0:51 may blow up. And so we can see here that you know the context window eventually fills up and the longer it runs the
0:58 longer you wait and sampling more doesn't either fix it because each attempt here still has to do the whole task on its own right. So what if we
1:07 split the work? Um um so here in this case the key the lead keeps the task and hands out subtask and each worker starts
1:15 with a fresh context. So it can read files, it can run tests, it can dig through logs etc etc and then it can send back only a compact result. So here
1:24 the lead keeps the full picture and it can finish faster if the task is decomposible and agents can work uh on independent pieces.
1:34 And so here we're talking about a new ax uh for scaling inference time compute.
1:38 You work as a team. You give a task to a team of coding agent and watch them do the task for you.
1:45 But then scaling the team gets expensive very very quickly. And so as you add more agents your token budget can blow
1:52 up um and it can actually um and it raises a harder question actually. So first how does coordination actually
2:00 happen? How should we organize the team so that agent can work together etc. And so you know this is the question that we
2:07 set out to answer. How should a team be orchestrated to be both effective and efficient? By effective I mean it actually solves more of the task so we
2:16 get higher reward and by efficient I mean it doesn't burn through your budget getting there.
2:24 All right. So since these models now are extremely capable I was wondering what if we throw an extremely challenging
2:32 coding task as a society of agent and see what they can do and so we would expect them to automatically form clean
2:39 groups split up the work neatly and build it together communicate nicely and coordinate etc etc but that's not what
2:46 not quite what happened actually in reality what happened the agent preferred to solve the task on their own so each went off and build their own
2:54 version. Even when we asked them to communicate, they mostly discussed things. Sometimes that didn't matter and sometimes they kept also stepping on
3:02 each other toes. They also prefer to communicate back to the lead etc etc. So here we were wondering okay how should
3:09 we design a team so that you know we can actually get the best out of it in terms of reward and in terms of cost and
3:17 hopefully in terms of latency. And so you know we set out to figure out what a multi- aent harness should actually look like and like we were wondering about
3:24 the team design. So there are many design choices and each one changes how well the team does and how much it
3:32 costs. So first the organization right how do you structure the team who leads and at which stage of the work planning
3:39 implementation or verification do actually workers come in. Um the second thing is communication. Should agent
3:46 talk to each other directly or only report to the lead? Third is the team size. How many workers is worth it? Like
3:54 should it be one, two, 16, 100 etc. And fourth the models, right? Which model should play which role? Do we need you
4:01 know a strong model everywhere or you know can cheaper ones do the same work right? And finally here that something
4:08 that we cared about is the stature of the software engineering right should should you know we should allocate more workers towards planning towards implementation toward verification etc.
4:20 And so we actually frame team design as a compute allocation problem. So what we cared about is reward or added dollar
4:28 over the solo agent. Right? So starting from a solo agent we asked where does the next dollar of inference buy the most reward. This is what we cared
4:36 about. And so here we actually define a metric called efficiency. And it's actually a function. Four things. First the stage. It's the the stage at which
4:45 the worker is at whether it's implementation, planning or verification or all of them together. We call it we also define that as a function of the
4:54 worker model. Is it like a very capable model? Either is it a very you know you know less capable model. The number of
5:01 workers, the lead model etc. Um and all right so basically if we to answer this
5:08 question we actually design our multi- aent harness as a top- down hierarchy.
5:12 We build deliberately a minimal harness laminu agent and it's a hierarchical as I mentioned here we have a lead and based on the task it decides dynamically
5:21 how big the team should be and um you know who should do what and so here we made sure that every part of the harness is exposed so we could change one choice
5:30 at a time we keep everything else fixed and see how each one moves reward and cost right and so here more closely how
5:38 does a team of coding agent do the work you can see here every agent works through a bash tool in one shared workspace. So basically the lead
5:46 receives the task and hands out work with a tool we call delegate agent and then the worker sees only its subtask and reports back to the lead and it can
5:55 also communicate directly to the other worker and so here basically the lead can decide based on the task oh actually I need to just you know pow an
6:03 implementation worker or the verification worker etc. And so as I mentioned also the lead decides how many workers to spout, how to combine also
6:11 consolidate the results. It also has the capability to resume a worker, to kill a worker and to tab in and for example
6:19 supervise and tell the worker that you have [snorts] made some errors here, fix it etc. And so yeah and you may be
6:28 wondering okay what is the lead doing while the other workers are working? We actually have two variation. One is what we call the blocking lead. The other one
6:35 is a synchronous lead. So the blocking lead actually hands out subtask and waits until every worker reports back.
6:42 It doesn't do anything you know in between. Uh whereas an async lead it keeps actually working while you know the results keep coming from the other
6:50 workers. It can supervise what other workers are doing. It can send them feedback etc etc. And so how did we test this? We test this on a benchmark called
7:00 program bench which is basically a reverse engineering task. So basically the agent starts from an empty workspace with only the original program
7:08 executable which it can run but not read and it also receives some documentation.
7:13 So here the goal is to basically probe the program to discover its interface to discover the edge cases the behavior and then write a new implementation that
7:22 behaves in the same way. And then the task here were extremely challenging.
7:26 They range from SQLite sorry for example the PHP interpreter and for example you know on the leaderboard on the official
7:34 website on the task fmp the best model only passes 10% of the test so this is
7:41 also done by you know frontier model so it was quite challenging and it was a good test bet for for our you know
7:48 investigation and so the question here is that does a team beat a solo agent yes or no the answer is yes it actually
7:55 can beat a single agent but at four to seven times the cost. And we can actually see here in the figure that you
8:04 know with a minimal prompt a team with a blocking lead actually beats the solo agent by almost 10 points but it quotes
8:11 about cost about four to seven times as much. Now you can see that the blocking actually performed better than the
8:18 asynchronous but cost higher and it actually takes more time to complete. So what we noticed here is that we noticed
8:25 that the asin clean doesn't know when to wait when its workers are still exploring. It starts building on its own
8:32 guesses before the workers finding come back and then when the reports finally arrive it has already moved on and so
8:40 either it ignores them or it has to go back and redo the work and sometimes it just decides the task is done while the
8:47 workers are still actually running. So basically it decides okay done it submit them work and so the work of these workers is simply lost but then you know
8:55 this problem never occurs with a blocking lead because it will wait until the workers report back and then it actually consolidates the work and do the submission.
9:05 All right so the next thing I want to emphasize on is the prompt minimal. So I I mentioned okay in the earlier results I we used the minimal prompt but is an
9:12 earlier prompt and I can't emphasize on this enough. This is how our agent operates. We gave them prompt and instructions and essentially they do
9:21 what we asked them. And so the first one we tried to go very very minimal. We just told the lead about the task you
9:28 know that it should create you know sub workers an agent and it should inform them about the task and should tell them
9:36 about the tools. We gave also the lead tools. We wanted to see why we started minimal because we wanted to see how the
9:43 agent behave on their own. Do they need a lot of guidance or is it is a minimal prompt enough etc. And so this is very
9:51 minimal. We also were wondering okay what happens if we give it a very heavy prompt meaning like you know what the model should exactly do. We tell it
9:59 exactly about different tricks. We follow exactly this plan of try to you know delegate agent and you know create agent throughout the whole process of
10:08 the implementation from exploring and planning to implementation. Create also adversarial workers verify the task etc.
10:15 etc. And actually that helped a lot. Uh we can see here um you know compared to
10:22 the minimal prompt um you know we can see that the solo agent alone jumps almost six point just from better instructions and telling them okay don't
10:31 step on each other toes don't you know uh there is this folder right in this folder etc. So lots lots of guidance and
10:38 lots of prompt tuning which is not ideal because we spent too much time there and now these agents are supposed to be smart enough to figure out how to do it.
10:46 And so we can actually see that the teams both you know async and blocking both go up. Um and we can see that the async lead now catches up with the
10:54 blocking one right. So once we tell the lead how to coordinate it knows when to wait. So this actually really helped address the problem that I talked about
11:02 earlier. But again, you know, we have a team that works better than the solo, but it's really, really expensive. It costs six to seven times as much as the
11:10 solo agent. So, we were wondering now that a team beats a solo agent, um, but it costs more. So, if a team is a new
11:17 way to spend inference time compute, where should the next dollar go?
11:22 Basically, can we actually optimize our agent setup so that actually we get very
11:29 good reward but at a very, you know, much cheaper cost. So to answer this question actually we took the same bead and gave it workers at only one stage of
11:38 the software engineering process. So the first setup that we considered here the workers are only responsible for the
11:45 planning and exploration right. So basically here the lead will give them the task and then each of the workers will go and try to prop the binary to
11:54 discover okay what kind of plan I need to do and then it will report the plan to the lead and the lead will take care of the implementation verification. The
12:02 second one is you know workers are only responsible for implementation and so the lead is the one responsible for coming up with a plan distributing that
12:11 splitting the work between workers. they will do the implementation report back with a code actually to the to the lead
12:18 and so basically and then the lead does the verification on its own. Now the third actually setup that we had the
12:26 lead is responsible for both the planning and the implementation and then here the workers are responsible to do the verification which is the final
12:35 stage and then they will report back to the lead and then the lead will decide whether to submit or not. And so okay we
12:42 wanted to see where exactly extra agent will pay off the most and here are the conclusion of our results like extra agent pay off most in program bench in
12:51 planning. So here you can see okay going from solo with minimal prompt to solo with PIV. PIV is the heavy prompt
12:59 actually we get like a jump already and then we go with a team of heavy prompting we get extra you know increase and gain in performance but then now
13:07 when we allocate more agent towards the planning right we can see that we get almost the same performance as the PIV
13:14 but at half the cost. So actually the conclusion here is that you know the extra agent pay off the most in planning
13:21 and um at at the task that bottlenecks actually uh the the the task which is
13:29 planning in this case because you need to figure out how exactly what's the binary behavior and come up with an detailed plan on what should you
13:37 implement first etc. And so here yeah I actually we computed even you know the efficiency ratio that I discussed earlier and we can see that you know it
13:46 dominates the PI actually on the frontier. So each planning dollar buys almost 1.6 more reward than any other
13:54 stage that we have here whether it's implementation or verification and actually we we we notice that you know you know doubling down on the number of
14:02 agents in the implementation st in the implementation or verification stage doesn't actually really help. We can see that it just increased the cost but we
14:10 are actually getting the same performance as the solo. So it's not worth it. And so again like program bench only exposes a blackbox
14:17 executable. So discovery here is the bottleneck. So planners paralyze this probing and it brings and they bring
14:24 lots of benefits right and so yes so this is to explain why planning workers pay off and we were wondering why not for example verification or
14:32 implementation and you know as I mentioned program bench give you this binary and its documentation so before you write anything you have to work out
14:40 what the program actually does so discovery is the real bottleneck here and you can also think about this in
14:47 terms of context right so we have one agent that's exploring, you know, loan files. It's in its own context with probing and also still misses edge
14:56 cases, right? And on the other hand, you know, we have several workers each explore a different part of the program in a fresh context and each one hands
15:04 back a compact report to the lead. And so basically here the lead, you know, learns more than it could do alone. And
15:11 so now you know this is also to explain better this phenomena and you can we actually looked at our data and we came
15:18 up with these nice figure based on what we saw and we specifically looked at when the lead delegates right. So here we have planning we have implementation
15:27 and verification and we have all the stages as well and so every row that we can see here in the figure is one run and actually time goes left to right.
15:37 Right. So the yellow is planning, the blue is implementation, the green is verification. And every dot here that you see in the figure is the lead
15:44 sending work to a worker. And so with planning guidance, all the delegation happens in one burst at the very very
15:51 first of you know the the work before any code is written. So basically here the the workers go explore and then they
15:59 report back and then the lead builds with the full um PIV guidance where you know um there is no um specific guidance
16:08 on where exactly the the the number of workers should be allocated. We get the same early burst, but then if you see
16:16 here, you actually the lead keeps delegating all the way through. Another 830 calls and they buy nothing.
16:22 Actually, it just increase the cost as we saw. It was around $500, but it didn't actually improve in performance.
16:29 So, it's the same reward for planning we can see, but it's 2.4 times more reward per call that we we actually measured.
16:38 And so what pays off here is is one early burst of discovery and not delegating all the way through
16:46 right so why implement workers fall short here first of all as I mentioned the plan was the bottleneck and so if you don't do it right it's not going to
16:54 work out and so here we have only the lead explorer it had limited capacity to discover all you know the the edge cases
17:01 and the full binary and so we had a weak foundation and then basically each worker got like a narrow slide it
17:08 doesn't have visibility over the entire task and basically they were already doing errors by implementing each
17:16 feature on on their own. That's one problem and on top of that they were building on a weak foundation and also we noticed that so here you might ask me
17:25 that you know why not give workers more context we did but we noticed that when you they have more context actually they
17:32 didn't respect the boundaries of their specific task they get excited and then they want to build extra features that nobody asked them for it so they just
17:40 step on each other toes etc. The other issue that we noticed is that you know they are supposed to communicate together to figure out who's doing what
17:48 to avoid conflicts and duplicated work but they didn't do that. They actually they worked alone and 84 of messages go
17:55 go to the lead directly and nobody sees who's doing what and sometimes here what happens is the lead gets overwhelmed and it doesn't know doesn't communicate well
18:04 where the problem is to the other workers etc. So it can be actually quite challenging. And so what happens here is that when you know implementation is not
18:13 fully correct, you know, the the the lead pays for the gap. It actually tries to, you know, make up for that duplicated work and does lots of
18:21 stitching. And so here what happens is that the latency goes up and also the reward goes down and you know it's also expensive.
18:31 Uh okay. So basically so far I've been talking about program bench but we also run experiments also on commit zero and
18:40 what I want to say here is that the result like are are different and the hierarchy and the the allocation of the
18:47 team is different according to the task because the bottleneck for program bench was planning and so allocating more agents there really really helped but
18:56 for comet zero it was not the case. the comet zero the bottleneck was not planning because basically you were given the skeleton so the dogs aren and
19:04 the unit tests are actually known to the workers so the plan is basically given to you and the the the bottleneck was
19:11 for the you know for the reward to go up was the implementation you need to get the implementation right and so here when we doubled the number of agent on
19:20 the implementation stage this is where we saw the biggest improvement I'm not going to present results on this please check out the paper for for for these
19:29 results. You'll find them in the appetics. So the conclusion here is that you need to allocate more agents at the software engineering stage that bottlenecks your task.
19:39 All right. So planning already uh gets us the same reward at half the cost. So uh we were wondering can we go even cheaper than this? Can we cut the cost
19:48 even further? And so here we did some experiments and we focus for example on three GPT 5.6 six uh models Luna, Terra
19:58 and Saul in this uh increasing order of capability and cost. And so like the question here is that what would happen
20:06 if we keep Saul as the lead like the strongest model and then use the cheap models as the workers, right? So
20:13 remember here we used to have this this is all the P here is the old sole you know model by the way here the the the
20:22 sole the basic this is the version like of P where the lead dynamically decides on the size of the team and it ended up
20:29 on average creating and spowing five agents right and so basically here we have this we go from PIV we actually
20:37 allocate more agent towards B and basically when we replace the sole, you know, subworkers with cheap ones like
20:46 Cloa, we actually get almost, you know, same performance but at 60% less the cost. Like just we lose a little bit on
20:54 performance, but it's still good because we're actually maximizing reward compared to the solo. And then we're
21:00 we're getting less cost. And so, you know, now the other way around, we were wondering like can the lead be cheap, right? We just even get a a cheaper
21:09 lead. And so already here we can see that okay you know going from like a cheap lead like solo Luna terasol we
21:17 keep increasing in performance and also in cost and so can we have like this homogeneous team like Luna plus Luna
21:24 Terra plus Terra already we can see the scaling law the same as the model gets more capable we get better results but
21:31 then actually you know we notice that when we have a strong lead Saul plus Luna we can get the same performance as
21:38 Saul plus Saul But you know at much lower the cost but the other way around like cheap workers can't make up for a week lead basically the lead sets the
21:47 ceiling and the workers set the price here terra with Luna workers still trail soul working alone 51% versus 57%.
21:55 So essentially here the lead sets you know basically this also matches meta's meta reasoning work recently like they
22:03 just mentioned that a better controller raised what the same workers achieved.
22:08 All right. So, so far the lead decided how many workers to spare as I mentioned and it did on average five workers. Now
22:14 we were wondering what would happen. We want to scale ourselves the number of agent at each stage. So we will impose
22:21 the team size. Um so here uh we're gonna you know work with soul as a lead luna as planners because this was the best um
22:30 reward cost um um uh relation and we go from two to 16 planners right um and so
22:38 here I can actually look at the cost you know the 16 planner team is 38% cheaper
22:45 than all soul planning team and actually 70% um cheaper than workers at every stage here for the P which is great
22:54 [snorts] because here we and it seems we we due to some budget constraint we couldn't like scale up the the number of agent further but it seems like going up
23:02 and up and up obviously there is a ceiling to it but it's really helping and the good news is that it's not that expensive it's not that it's not more
23:11 expensive than the five solen uh allocated towards planning now we also
23:18 uh try to see whether uh scaling up the number the size of uh the team for the other variants like implementation and verification and it didn't help at all.
23:28 Uh we already saw that uh you know allocating teams um agents toward in these stages uh is not helping because
23:35 of you know the foundation was weak and other considerations. And so the last thing we were actually curious about how do agents communicate, right? And so we
23:44 gave every agent a send message tool to reach any other agent actually in the send box. And we were curious whether
23:52 communication uh happens directly or you know it happens through the lead and whether communication will lead to better performance or worse. And so
24:01 basically the first thing is that are they communicating together? Not really.
24:05 They are actually essentially commun communicating through the lead like 84% of all messages went from a worker to the lead. Worker to worker you can see
24:14 is actually very little less than one message per task. So even when they can talk to each other they actually behave like a hierarchy like do your piece your
24:23 report app expect the lead to communicate back to the right sub agent etc. And you can also see this pattern consistent across all models. The lead
24:31 takes much more talks sorry talks much more to workers than they talk to to each other. And this is also true for cloud office 4.7 also we did experiments
24:41 on Gemini etc. And so also as I mentioned we were curious about the reward does it you know does extra
24:48 communication between workers help or not in our experiments we didn't find it helps and I will explain why. Um so
24:56 first already you can see you know on the left you can see that about half of Luna messages never even get through like because they're malformed and the
25:04 harness reject them etc. Um, and so we notice also in the middle figure that Saul writes about three to four times as
25:12 much message text as as Luna and under the PIV guide and stronger teams actually we can see that they tend to
25:19 send more messages overall with leads steering their workers more which is good. So we can see that you know as the you know capability of the model increases so they message rather more.
25:30 But then on the right figure you can see that more talking doesn't mean more reward. For example, for Sol and Tora
25:36 Terra, adding messaging changes nothing basically beyond noise and Luna teams actually lose four to seven points. So,
25:44 it's just seems distracting to them. And we noticed that communication mostly carries status report to a lead, you know, who already receives each worker
25:52 results anyway. So, it it was just like an overhead. There was nothing important to be discussed. And I want to be clear here. I don't want to say that peer
26:01 messaging is not necessary and we should not do it. I do think it's necessary and I do think as we scale the number of agent they should
26:09 be capable to communicate efficiently together because it doesn't make sense to all the time communicate through your manager for you to work with your team members. No, it doesn't work like that.
26:18 I think that it's also a byproduct of how of the harness that we built and also of the you know the task itself
26:26 because basically the lead was splitting the work and making sure that each work is independent of the others and so they
26:34 give them the lead gives them directly the task and so they are not also aware of other you know agent in the same
26:41 sandbox and so we gave them the tool only to communicate together but they didn't have visibility who exists there and so they have
26:48 do extra efforts and maybe a message word would have solved the problem and so would incentivize them more to sort
26:55 of communicate and to figure out oh this actually agent is doing something similar to what I'm doing let me communicate or I need more help let me
27:03 reach out to someone else anyways we leave this to future work and so the other thing that we also were curious
27:09 about is like you know whether other you know models frontier models know how to lead so we also run have other models
27:18 family with the same harness with identical tools and so here I'm talking about oppus 4.8 4.75 also Gemini family
27:27 we noticed that GPT 5.6 six teams delegates often and it gained up to seven points in reward. CL on the other
27:35 hand actually behaved much you know the same gaining almost six points but then oppus 4.7 rarely delegated and actually
27:43 gained very little haiku and gemini you know delegated as red lead as GPT 5.6 six but then gained nothing and so what
27:51 I want to say here the conclusion is that you know two things have basically to to go right the lead has to choose to
27:58 delegate this is important because it's actually deciding on who's going to do the work and it has to actually make use of what comes back and at which stage of
28:07 the task you also delegate all right so here are the takeaways three things to remember the first thing is that teams
28:15 are now a real new for inference time comput but the catch here is that they cost several times more. We got up to 10
28:23 points which is you know great but still it's very very expensive. Now you know we tried our best to optimize our teams
28:32 of agent and we were successful to some extent still some work to be done but essentially here the conclusion is that
28:40 you know where you put agent uh matters more than how many. So you need to find the bottleneck and scale the team there.
28:49 This is what matters the most. And finally, the team does not have to all be strong. Like a strong lead with cheap workers can get you near frontier
28:56 results at actually a fraction of the cost.
29:00 And so what's next here? These are open questions that we are so curious and excited about. First here we're excited
29:08 about this hierarchy versus self organization. We in our work we tested up to 16 workers in a hierarchical lead worker structure. But then how should
29:17 teams of thousands of agent be organized, communicate and spend compute? Can agents design the team themselves given task budget and
29:25 deadline? We think so. And so but that requires you know careful investigation of the problem. Now the second thing is
29:32 that we think that there must be alternative forms of communication. As I mentioned we observed 84% of messages going from workers to the lead and only
29:41 4% between workers and so as I think project outgrow like coding project I know they can out go grow one lead well
29:51 effective communication here emerge right will scale or like should it be trained right how should the lead adapt
29:58 to the task it's not scalable to just for each task that we have we spend uh you
30:06 a huge amount of time optimizing the prompt and giving very detailed instruction to the lead. That should not be the case. The lead should figure out
30:14 how to actually you know uh organize the team uh according to the task that it has. Um now finally uh multi-agent
30:23 safety and security um you know uh this is I can't emphasize on this enough especially given what happens with the
30:31 hanging face incident. You know, right now we have teams of agents that can run autonomously 24/7 with no supervision.
30:41 They're quite capable and we're not talking anymore about a single agent, but we talking about teams of agent and even swarm of agent. And so here the the
30:50 key question is that how do we secure our systems and our infra and we need to
30:57 urgently basically figure out ways on how to align these models. All right, with that, I actually would like to
31:05 thank you for making it to the end. And I want to give a special shout out to the first author here, Kenan and Daniel, for their hard work on this. And for
31:13 more information, please check out the paper and the blog using these QR codes. And thank you so much for listening.
