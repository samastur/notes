# Social Signal Processing - Understanding Social Interactions Through Nonverbal Behavior Analysis

[Alessandro Vinciarelli](http://www.dcs.gla.ac.uk/vincia) (University of Glasgow and Idiap Research Institute)

* nonverbal communication (== social signal)
* example of silhouette showing how we can interpret even situation even when missing most of information (from posture, mutual gaze, vocal behaviour, interpersonal distance, gestures == nonverbal behaviour cues)
* main elements
	* cues (clothes, expressions (gaze), vocalisations, self-touching, distances...)
	* codes (physical appearance, face behaviour, voice behaviour, gestures and postures, space and environment)
	* functions (forming impressions, expression (emotion), relational messages, deception management, power persuasion)
* what happens if we replace person on the left (giving cues) with a machine?
	* voices in navigation software are carefully selected to sound nice but authoritative
* can we generate signals that convey the same social functions? (synthesis problem)
* if we replace receiver with machine, then we have analysis problem (how to interpret signals)
* The Conflict Example
	* definition: conflict is a *mode of interaction* where the *attainment of the goal by one party precludes its attainment by the others*
	* mode of interaction: physical layer that we can detect (sense)
	* inferential layer is what can't be observed, but has to be inferred from context
	* example: political debates (provide good data)
* SSPNet conflict corpus (Swiss Canal9 debates in total length of 11h 55m)
* used mechanical turk to assess those 30 second clips; two types of questions: mode of interaction (how people behaved) and those about inferential layer
* at least 10 people (not speaking french used in clips) had to fill the survey for each clip and from those they averaged out two scores: physical and inferential
* these two scores should be correlated; they are
* speech features
	* length of talks
	* when competitive, everybody starts to talk and you get more equal speaking time
	* do they talk over each other
	* how fast do people react to others (more quickly when you strongly disagree)
	* pitch (higher when tensed) and loudness
* goal to automatically predict the conflict level
* in models what they found out is that minimum level were usually not outliers, but already enough for factor to matter (explanation: our perception is driven by moving beyond thresholds not absolute values - e.g. loud enough)
* The Personality Example (how can we infer personality of a person from speech)
	* personality is the *latent construct* accounting for individuals' characteristic patterns of thought, emotion and behaviour together with the psychological mechanisms - hidden or not - behind those patterns
	* big five personality factors/traits (chosen model):
		* extraversion (active, assertive, energetic, outgoing)
		* agreeableness (appreciative, forgiving...)
		* conscientiousness (efficient, reliable...)
		* neuroticism (anxious, self-pitying, tense...)
		* openness (artistic, curious, imaginative, insightful)
* data came from people talking on Swiss radio in February 2005 (322 subjects, 640 speech clips, 10 seconds per clip)
* when we hear people we immediately attach personality to them even without realising
* survey: 10 questions each of them associated with one of the traits
* assessors where 11 British people not speaking French
* worked best on aspects that are important for speaking on radio (extraversion and conscientiousness)
* example of conscientiousness: what drives perception is entropy (variability)
* more information at [SSPNET](http://wwww.sspnet.eu)
* conclusions
	* nonverbal communication is a physical machine detectable evidence of social and psychological phenomena
	* interdisciplinary approaches can perform better (including both human and computing sciences)
	* real world applications are the next frontier in terms of data, problems and methodological issues
	
### Questions
* how do you find relevant traits? start with psychological interaction and deconstruct psych. phenomena involved, but no step-by-step guide (helps to talk to psychologists)
