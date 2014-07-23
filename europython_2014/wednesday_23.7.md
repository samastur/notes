# Wednesday, 23.7

## Our decentralized future (Pieter Hintjens)
* Pieter: fighter agains software patents, ZeroMQ developer, book writer
* ZeroMQ first community he has been in where there is no fighting (few flame wars), which is unusual in large projects
* ZeroMQ does not do design, meetups to discuss what will come, no wishlists, no irc...occasionally have beer
* it is not accidental, because they developed over time a process of working which was to move away from centralization
* world we live in is about decentralization, giving up control (food supply for a city has nobody in charge)
* so why do we accept centralization in our software projects?
* ZeroMQ community mimicks their tool (async, decentralized, no locks...)
* the future of computing is distributed if we like it or not; you won't have a job if you will not be able to build for this world
* how do we build decentralized systems? we can't, we grow them organically
* internet is a set of protocols (RFCs), a set of contracts
* most of the work was done without actual real fighting at all (??)
* alternatives had good developers producing good software, but failed
* what makes good software? what is quality in software? (as industry we don't understand this)
* common way of developing with main authors and maintainer who reject patches they don't like; what was even worse than feeling of rejection was that stuff coming out wasn't very good either
* a lot of code in old ZeroMQ wasn't used because of wrong opinions and expectations of first author(s)
* so the current process is you send a pull request to master and Pieter will merge it (there's a little bit of filtering for sanity, but not much); maybe their patch is not the right patch, but their presence is the right presence
* the quality of patch is measured not by its beauty, but how accurate and relevant it is; Wikipedia was the model for development approach
* only market can write the software that the market needs
* software we build will reflect structure of the organization that built it
* how do you come back to quality with ZeroMQ approach? (question from the audience)
	* allow mistakes, but work on quickly fixing them (let people learn from them)
	* if you can make mistakes quickly, you can also fix things quickly
	* so all work is done on master (they make stable branches for customers, but don't use them)
	* they let users contribute test cases
	* contracts need to be testable
	* every patch triggers test run so they know quickly if it broke anything
* another question: benevolent dictators in early stages?
	* in software we don't have that much controversy (?!); if there are it's good if they are documented and in software controversies are "stored" in forks and market picks the winner
	* his role is mostly being lawyer; use it, write about it (RFC) that community can use; he ignores his vision because it's junk
* question: this outlook feels anarchic (in political sense); how did this outlook come about?
	* you have authority in anarchy, but anybody is free to chose the one they like best
	* there is and has to be authority because who will enforce contracts (people will cheat); there is always a small percentage of dedicated cheaters and they will do great damage to communities
	* he had bad experience with people like that in FFII where he wanted to run by consensus
* question: you can have power without realizing or accepting that you do (white male); how does such process take into account diversity?
	* he can't talk about diversity, but can talk about work done to remove obstacles to contributing
	* he thinks the process is competitive and may have certain communication patterns which may lead to most contributors being male
	* he's an Adam Smith type believer in free market; differences are valuable
	* he accepts that he is biased, but he can't see it (he suspects that gender bias is not the biggest one; there's age bias, race...)
* question: do you think people will chose the best software for their own benefit?
	* what is most interesting in software is chosing the right problems and solving the right (biggest) problems first
	* what ZeroMQ community tries to do is to identify those problems
	* it is fundamentally up to users to decide what is right for them; he can't
* question: is this a different management style? what about the size of community?
	* he doesn't think it's about scale; you have to decide what kind of approach you want to use