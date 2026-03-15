---
title: "Adoption Patterns of AI-Assisted Software Development Tools"
date: 2026-03-14
description: "Historical precedents and change management strategies"
tags: ["ai", "software-development", "change-management", "history"]
draft: false
---


There is a recurring pattern worth examining: when transformative technology enters an established field, incumbent experts resist while adjacent professionals adopt. The pattern has repeated across medicine, surgery, textiles, printing, music, and accounting. It is now repeating in software development.

Experienced developers adopt AI-assisted tools more slowly than product managers, analysts, and junior engineers—not because they are resistant to change, but because they have more to lose. Their resistance often reflects legitimate quality concerns, hard-won expertise under threat, and professional identity bound up in craft. Understanding these dimensions is essential for anyone leading teams through this transition.

The article introduces a framework distinguishing three types of resistance: **rational caution** (grounded in current, accurate assessment of tool limitations), **stale caution** (based on outdated experience with earlier-generation tools), and **identity-based resistance** (categorical rejection rooted in what adoption implies for professional self-concept). Each requires a different intervention.

Beyond the current conversation about code completion and IDE-based assistance—what might be characterised as Stage 1-2 of an eight-stage progression—practitioners at the frontier orchestrate dozens of parallel AI agents, operating in a fundamentally different mode. Two maturity frameworks emerge:

- **Individual Evolution:** Eight stages from basic autocomplete to orchestrated multi-agent workflows
- **Team Maturity:** Five levels from experimentation to full autonomy

A critical observation: **AI coding tools are improving faster than mental models are updating.** Many developers carry assessments formed on 2023-era tools into a 2026 reality. This creates a specific intervention opportunity—not arguing about past experience, but inviting fresh evaluation.

There is also an uncomfortable truth here: the traits that made someone excellent at traditional coding may work against them in AI-augmented work. The role is transforming from maker to evaluator, from deep focus to context-switching, from deterministic thinking to comfort with probabilistic output. This transition will not suit everyone.

The article concludes with a dual-track change framework addressing both individual progression through stages and organisational maturation through levels, with specific guidance for leaders navigating this transition.

---

# Part I: The Historical Pattern

## 1. Introduction

Every technological transformation produces the same peculiar outcome: the people closest to the work resist longest, while those at the margins adopt first.

When hand-washing was proposed to reduce maternal mortality, senior physicians rejected it while midwives and nurses adopted. When anesthesia arrived, surgeons resisted while dentists embraced it. When power looms threatened textile workers, the most skilled craftsmen led the resistance. When spreadsheets transformed accounting, senior partners warned of lost understanding while junior analysts seized the new tools.

The pattern is now repeating in software development. In most organizations, the professionals *most* invested in the craft of coding—experienced developers with years of accumulated expertise—adopt AI-assisted tools more slowly than product managers, business analysts, and junior engineers.

This observation is frequently misunderstood. Leaders interpret slow adoption as resistance to change, technophobia, or protection of turf. These explanations are rarely adequate. The historical record suggests something more complex: expert resistance often reflects genuine insight into a technology's limitations, legitimate concern for quality standards, and identity bound up in hard-won skill. The resistance is not irrational—though it is also not always correct.

Three questions are worth addressing:

1. **Why does this pattern keep repeating?** What are the underlying dynamics that cause experts to resist while adjacent professionals adopt?

2. **What distinguishes legitimate concerns from outdated ones?** How can leaders tell the difference between rational caution and reflexive rejection?

3. **How should organizations navigate this transition?** What frameworks help individuals progress and teams mature without leaving wreckage behind?

The stakes are significant. Organizations that dismiss expert concerns as mere resistance will alienate their most capable people and likely suffer quality consequences. Organizations that defer entirely to skepticism will fall behind as the technology matures. The challenge is discernment: which concerns deserve respect, which deserve fresh examination, and which reflect something deeper than tool capability.

---

## 2. Historical Precedents

### 2.1 Medicine: Semmelweis and the Psychology of Rejection

In 1847, [Ignaz Semmelweis](https://en.wikipedia.org/wiki/Ignaz_Semmelweis) was a Hungarian physician working at Vienna General Hospital's First Obstetrical Clinic. The clinic had a troubling distinction: a maternal mortality rate of 10-35%, compared to 2-4% at the adjacent Second Clinic staffed by midwives. The disparity was well known. Women begged to be admitted to the Second Clinic. Some gave birth in the street rather than enter the First.

Semmelweis investigated systematically. The turning point came when his colleague Jakob Kolletschka died after a student accidentally cut him during an autopsy. Kolletschka's autopsy findings resembled those of women who had died from childbed fever. Semmelweis recognised the connection: doctors were carrying "cadaverous particles" from the autopsy room to the delivery room on their unwashed hands.

He instituted mandatory hand-washing with chlorinated lime solution. Within months, mortality at the First Clinic dropped to 1-2%, matching the Second Clinic where midwives—who did not perform autopsies—had always achieved better outcomes.

The medical establishment's response was not enthusiasm or even skepticism. It was hostility.

Professor Carl Braun, who succeeded Semmelweis at the First Clinic, proposed thirty alternative explanations for the mortality difference. The prestigious *Vienna Medical Weekly* dismissed Semmelweis's findings. [Charles Meigs](https://en.wikipedia.org/wiki/Charles_Delucena_Meigs), an influential American obstetrician, declared that doctors were gentlemen, and a gentleman's hands could not transmit disease. Semmelweis was forced out of Vienna, his career destroyed. He died in an asylum in 1865, possibly beaten by guards, from an infection.

The resistance was not stupidity. The Viennese medical faculty included some of the most educated people in Europe. The resistance was psychological. Accepting Semmelweis's evidence meant accepting that doctors themselves—healers by identity and vocation—had been killing their patients. Not through malice or incompetence, but through the very practices that defined their profession. The psychological cost of this recognition was unbearable.

What Semmelweis threatened was not merely a practice but an identity. The senior physicians had built careers, reputations, and self-understanding on existing methods. To accept that those methods were deadly was to accept that their life's work had caused harm. Denial was not irrational in any simple sense; it was self-protective.

The pattern is significant: those who adopted first—midwives, nurses, younger doctors—had less identity invested in the old practice. They could see the evidence without it destroying their self-concept. The resistance was strongest precisely where the identity investment was greatest.

### 2.2 Surgery: When Expertise Becomes Liability

On October 16, 1846, [William Morton](https://en.wikipedia.org/wiki/William_T._G._Morton) demonstrated ether anesthesia at Massachusetts General Hospital. A patient underwent surgery without screaming in agony. The event is commemorated as ["Ether Day."](https://en.wikipedia.org/wiki/Ether_Day)

Within months, anesthesia had spread to Europe. Within years, it had transformed surgery. The age of painless operations had arrived.

Yet many elite surgeons resisted.

The resistance appears inexplicable from a modern perspective. Who would oppose eliminating surgical pain? But the resistance becomes comprehensible when I look at what anesthesia threatened.

Elite surgeons of the mid-nineteenth century had built their reputations on speed. [Robert Liston](https://en.wikipedia.org/wiki/Robert_Liston) of London was legendary for his rapid amputations—a leg in 28 seconds, a patient told "Time me, gentlemen!" as surgery began. This speed was not mere showmanship. Before anesthesia, patients were conscious and restrained. The longer the surgery, the greater the trauma, blood loss, and shock. A fast surgeon was a humane surgeon.

An apocryphal story, likely originating in Richard Gordon's 1983 book [*Great Medical Disasters*](https://www.google.com/books/edition/Great_Medical_Disasters/0v99O3QESPUC), claims Liston once amputated so fast that he severed his assistant's fingers and slashed a spectator's coat, with all three later dying—the patient and assistant from gangrene, the spectator from fright. Whether true or not, the story's persistence reflects a genuine reality: speed was the paramount surgical virtue, and Liston was its supreme exemplar.

Anesthesia made speed irrelevant. A surgeon could now take time, be careful, work methodically. The skill that had distinguished Liston and his peers—decades of practice achieving rapid technique—was suddenly valueless. Worse, the slower surgeons they had looked down upon were now their equals.

Adding to the resistance, early anesthesia was genuinely dangerous. Patients died from overdoses, aspiration, and cardiac arrest. Surgeons who warned of these risks were not wrong—they were accurately assessing the technology's limitations. But some generalized from these accurate concerns to categorical rejection: "Anesthesia will never be safe enough."

[Alfred Velpeau](https://en.wikipedia.org/wiki/Alfred_Velpeau), an influential French surgeon, declared in 1839: "The escape from pain in surgical operations is a chimera. It is absurd to go on seeking it... knife and pain are two words in surgery that must forever be associated."

Forever, he said. Anesthesia arrived seven years later.

The adoption pattern is instructive. Dentists adopted almost immediately—their patients could simply refuse painful treatment, and dental procedures carried lower mortality risk. Obstetricians followed, driven by patient demand for painless childbirth. Surgeons came last, and only after anesthesia techniques improved enough to address their legitimate safety concerns.

### 2.3 Textiles: When Craftsmen Burned the Future

The [Luddite movement](https://en.wikipedia.org/wiki/Luddite) of 1811-1816 has become a byword for irrational technophobia. To call someone a "Luddite" is to dismiss their concerns as backward-looking and futile. The historical reality deserves more careful examination.

The Luddites were not ignorant laborers fearful of machinery they did not understand. They were highly skilled artisans—croppers, stockingers, weavers—who had completed seven-year apprenticeships to master their craft. A skilled cropper could feel cloth variations invisible to the eye. A master weaver could produce intricate patterns from memory, maintaining consistent quality across hundreds of yards. These were craftsmen whose expertise represented a lifetime's investment.

The power loom threatened to make that investment worthless. Machines operated by children could produce cloth faster and cheaper than master weavers. The quality was initially inferior, but adequate for many purposes. The economics were transforming.

In March 1811, framework knitters in Nottingham began destroying the stocking frames that were displacing their work. The movement spread across the English Midlands and North. At its peak, more British soldiers were deployed against Luddites than were fighting Napoleon in the Peninsular War. The government's response was ferocious: frame-breaking was made a capital offence, dozens were hanged or transported to Australia, and the movement was crushed by 1816.

The tragedy of the Luddites is that they were partially right. Many did lose their livelihoods permanently. The master weavers who were 45 years old in 1811 did not retrain as factory workers; they spent their remaining years in poverty. The economic promise that "new jobs will be created" is true in aggregate and cold comfort to the individuals whose skills have been devalued overnight.

But they were wrong about the technology itself. Mechanized textile production eventually raised living standards broadly. Their children and grandchildren lived better lives because of the machines they had tried to destroy. The Luddites were right about their own futures and wrong about the future.

### 2.4 Printing: The Abbot Who Praised Handwriting

When [Johannes Gutenberg](https://en.wikipedia.org/wiki/Johannes_Gutenberg) printed his [42-line Bible](https://en.wikipedia.org/wiki/Gutenberg_Bible) around 1455, he sparked a transformation that would reshape European civilization. The immediate response from those whose work was threatened was resistance.

Medieval scribes were not mere copyists. Many were monks for whom copying sacred texts was itself an act of devotion. The scriptorium was a sacred space. Each manuscript was unique—illuminated, annotated, a work of art as much as a text. The scribe's craft involved not just accurate copying but knowledge of textual traditions, Latin scholarship, and artistic skill.

[Trithemius](https://en.wikipedia.org/wiki/Johannes_Trithemius), the Abbot of Sponheim, wrote [*De Laude Scriptorum Manualium*](https://en.wikipedia.org/wiki/De_laude_scriptorum_manualium) ("In Praise of Scribes") in 1492, arguing that printed books were inferior:

> The printed book is made of paper and will soon disappear entirely. The scribe working with parchment ensures lasting remembrance.

> Printed books will never equal the beauty of handwritten manuscripts. The dedicated monk does not seek speed, but excellence.

> The work of copying itself disciplines the mind and cultivates devotion. This spiritual benefit is lost when text is merely mechanically reproduced.

The arguments were not frivolous. Trithemius correctly observed that early printed books lacked the beauty of illuminated manuscripts. He correctly noted that paper was less durable than parchment. He accurately identified a spiritual dimension to scribal work that mechanical reproduction could not replicate.

He was also on the wrong side of history. By 1500, an estimated 20 million books had been printed in Europe. The scribes who could not adapt were displaced. But notably, many of the most successful early printers were former scribes who brought their knowledge of letterforms, layout, and textual accuracy to the new medium.

The irony was not lost on observers: Trithemius had his defence of handwriting printed. Even he recognised that the reach printing provided was essential for his arguments to be heard.

### 2.5 Music: The Synthesizer Wars

In 1984, the British Musicians' Union attempted to ban synthesizers from recordings. The union called for a "keep music live" campaign and proposed that recording studios be required to hire minimum numbers of traditional instrumentalists.

The underlying concern was real. Session musicians were losing work. Where a film score might once have required a 60-piece orchestra, a composer with synthesizers could produce comparable sounds at a fraction of the cost. [Vangelis](https://en.wikipedia.org/wiki/Vangelis) scored *Blade Runner* (1982) almost entirely with synthesizers. The economics of recorded music were transforming.

A union official stated: "The synthesizer is not a musical instrument. It is a machine that simulates musical instruments. Using it is not musicianship; it is button-pressing."

The ban failed completely. The technology was too powerful, too flexible, too cheap. More significantly, audiences proved indifferent to how sounds were produced. They cared whether they liked the music, not whether it was "authentic."

The adoption pattern followed familiar lines. The earliest enthusiastic adopters of synthesizers were not traditional musicians but producers and sound designers—professionals technically adjacent to performance who saw synthesizers as expanding their palette rather than threatening their identity. Traditional keyboard players initially dismissed synthesizers as toys, then gradually incorporated them as the instruments improved and the stigma faded.

### 2.6 Accounting: When Spreadsheets Arrived

When [VisiCalc](https://en.wikipedia.org/wiki/VisiCalc) launched in 1979, followed by Lotus 1-2-3 in 1983 and Microsoft Excel in 1985, bookkeepers and accountants faced a transformation of their work.

Tasks that had required hours of careful manual calculation could be completed in minutes. A financial model that once demanded meticulous pencil work—where changing one assumption meant erasing and recalculating dozens of dependent figures—could now propagate changes instantly. What-if analysis that was previously impractical became routine.

Senior accountants raised concerns that proved partially valid:

- *"Spreadsheets are error-prone."* True—spreadsheet errors have caused billions in financial losses, and research consistently shows that a significant percentage of complex spreadsheets contain material errors.
- *"The black-box nature makes auditing difficult."* Also true—following the logic of a complex spreadsheet is genuinely harder than following hand-calculated worksheets.
- *"Junior staff don't truly understand the underlying accounting."* Often true—the ability to build a working model does not guarantee understanding of accounting principles.

One senior partner at a Big Eight firm observed: *"The computer gives you the answer, but it doesn't give you understanding."*

Despite these concerns, resistance was futile. Spreadsheets were simply too powerful. A financial analyst with Lotus 1-2-3 could build models that would have taken a team of clerks weeks to calculate. Business managers who had depended on accounting departments for financial analysis could now do it themselves.

The profession did not disappear; it transformed. The number of people employed in accounting-related roles actually increased over the following decades, but the nature of the work changed. Manual calculation gave way to analysis, modeling, interpretation, and oversight. Bookkeepers who could not adapt were displaced; those who could became more valuable.

### 2.7 The Pattern

| Domain | Technology | Expert Concerns | What Experts Missed | First Adopters |
|--------|------------|-----------------|---------------------|----------------|
| Medicine | Hand-washing | Threat to professional identity | They were causing the harm they sought to prevent | Midwives, nurses, junior doctors |
| Surgery | Anesthesia | Real dangers; devaluation of speed-based skill | Techniques would improve; patient welfare supersedes technique | Dentists, obstetricians |
| Textiles | Power looms | Genuine economic displacement | Long-term productivity gains; descendants' prosperity | Factory owners, entrepreneurs |
| Printing | Press | Loss of sacred craft; inferior durability | Transformative reach; democratization of knowledge | Scholars, merchants, the Church |
| Music | Synthesizers | Loss of session work; "inauthentic" sound | New creative possibilities; audience indifference to production method | Producers, sound designers |
| Accounting | Spreadsheets | Error-prone; loss of fundamental understanding | Evolution of the profession toward higher-value work | Financial analysts, business managers |

Five observations emerge consistently:

1. **Identity threat cuts deepest.** Experts resist most strongly not when their jobs are threatened, but when their sense of self is threatened. The Viennese doctors could not accept Semmelweis because doing so meant accepting they had been killing patients—not through malice but through the very practices that defined their profession. The master weavers did not merely lose income when they burned looms; they lost their understanding of what made them valuable. The scribes who opposed printing were defending not just employment but a way of life organised around the sacred act of copying texts.
2. **Early versions are often genuinely inferior.** Expert skepticism frequently reflects accurate assessment. Early anesthesia did kill patients. Early spreadsheets did contain catastrophic errors. Early power looms did produce inferior cloth. The skeptics were not wrong about the technology's current state; they were wrong about its trajectory.
3. **Adjacent adopters move first.** Those close enough to understand the technology but without identity investment in the old way adopt earliest. Dentists for anesthesia, producers for synthesizers, analysts for spreadsheets.
4. **Resistance delays but rarely prevents.** Technologies offering order-of-magnitude improvements eventually win adoption regardless of expert resistance. The question is not whether but when, and at what cost during the transition.
5. **The transition is brutal for those caught in it.** Even when long-term outcomes are positive, individuals whose skills are devalued often suffer permanently. The 45-year-old master weaver does not become a factory supervisor; he spends his remaining years in poverty.

---

## 3. The Anatomy of Resistance

When a professional resists new technology, I find that the stated reasons rarely capture the full picture. Understanding what is actually being protected is essential for effective intervention.

### 3.1 Professional Identity

The deepest form of resistance stems from identity threat. When technology implies that hard-won skills are less valuable than believed, it threatens not just livelihood but self-concept.

The Viennese doctors could not accept Semmelweis because doing so meant accepting they had been killing patients—not through malice but through the very practices that defined their profession. The master weavers did not merely lose income when they burned looms; they lost their understanding of what made them valuable. The scribes who opposed printing were defending not just employment but a way of life organised around the sacred act of copying texts.

For software developers, code is not merely a product. It is an expression of thought, a craft requiring years to master, a domain where expertise is demonstrated through the quality and elegance of one's work. The suggestion that an AI can produce acceptable code challenges not just productivity but identity: *If a machine can do what I do, what am I?*

### 3.2 Sunk Cost in Expertise

Professionals invest years acquiring expertise. A developer who spent five years mastering system design patterns, memory management, or concurrency models has made a substantial investment. If AI can produce "good enough" solutions without that hard-won knowledge, the investment appears wasted.

This is not purely psychological. There are genuine economic implications. If AI enables junior developers to produce code that previously required senior expertise, the wage premium for that expertise erodes. The resistance is not merely about feeling undervalued; it may reflect accurate assessment of economic threat.

### 3.3 Quality Standards and Craft Pride

Experts have refined taste. They perceive flaws that novices miss. When senior developers criticize AI-generated code as "sloppy" or "cargo-cult," they are often making accurate observations: the code works but is inefficient, handles the happy path but fails on edge cases, follows patterns without understanding why those patterns exist.

The question is not whether these observations are correct—they often are—but whether "good enough" is acceptable for the use case. This is a judgment call that varies by context, and experts may apply standards appropriate for critical systems to situations where those standards are not required.

### 3.4 Loss of Control and Understanding

Professionals value control over their work product. A carpenter using hand tools knows exactly what each cut will do. A surgeon with a scalpel receives direct feedback from their hands. An accountant who built a spreadsheet formula by formula understands every dependency.

AI introduces opacity. When GitHub Copilot suggests a function, the developer does not fully understand why that particular suggestion was made or what edge cases might not be handled. This loss of control is especially uncomfortable for those who are held accountable for the output. "I don't know why it works" is an uncomfortable position for a professional.

### 3.5 Status and Hierarchy

Expertise creates authority. Senior developers have earned their position through years of demonstrated competence. If AI democratizes the ability to produce working code, it flattens the hierarchy that expertise created.

This parallels why doctors resisted hand-washing: the implication that nurses and midwives had been right all along—that their superior outcomes were not coincidental—was a status threat as much as an identity threat.

### 3.6 The Nature of the Work Itself

Many professionals genuinely enjoy the work that AI threatens to automate. The satisfaction of solving a tricky algorithmic problem, the flow state of writing clean code, the intellectual challenge of debugging—these are not merely means to an end. They are sources of meaning and pleasure.

If AI handles the "interesting" parts and humans are left with oversight, review, and error-correction, the work may become more productive but less satisfying. A surgeon who valued the craft of speed might find anesthesia-enabled surgery less rewarding even if outcomes improved.

### 3.7 Economic Displacement

The Luddites were right to worry about their jobs. Many did lose their livelihoods permanently. The fact that the textile industry eventually created more employment than it destroyed provided no comfort to those displaced.

Developers have watched other professions transform: travel agents, switchboard operators, typographers, bank tellers. The fear of displacement is not irrational. The question is whether AI-assisted development follows the pattern of displacement or augmentation—and the honest answer is that different roles may follow different paths.

### 3.8 Cognitive Load of Relearning

Expertise involves automated judgment. An experienced developer does not consciously think through every design decision; they have internalized patterns that guide them efficiently. AI-assisted development requires building new patterns: when to trust output, when to verify carefully, how to prompt effectively, how to review generated code efficiently.

This cognitive relearning is genuinely costly, especially for mid-career professionals whose mental models are deeply ingrained. The junior developer learning AI-assisted workflows from the start may find it natural; the senior developer must unlearn established habits while learning new ones.

---

# Part II: The Quality Question

## 4. Distinguishing Legitimate Concerns from Outdated Ones

Much resistance to AI-assisted development is framed as a concern about quality. Sometimes this concern is legitimate; sometimes it is rationalised identity protection; sometimes it is an accurate assessment of earlier tools applied inappropriately to current ones. Distinguishing between these is essential.

### 4.1 Concerns That Were Legitimate (2023-2024)

Early AI coding assistants had documented limitations that justified significant caution:

- **Hallucinated APIs.** Models confidently generated calls to functions that did not exist, referenced deprecated APIs, or invented plausible-sounding method names. Developers learned through painful experience to verify every suggestion against actual documentation.
- **Edge Case Failures.** AI excelled at "happy path" code—the main flow that handles expected inputs. It routinely failed on boundary conditions, null handling, error propagation, and exceptional circumstances. Production-grade robustness required substantial human revision.
- **Security Vulnerabilities.** A [2023 Stanford study by Perry et al.](https://dl.acm.org/doi/10.1145/3576915.3623157) found that developers with AI assistance produced code with *more* security vulnerabilities than unassisted developers, while *believing* their code was more secure—a concerning combination. The study documented patterns of SQL injection vulnerabilities, cross-site scripting openings, and inadequate input validation in AI-generated code.
- **Cargo Cult Patterns.** Generated code often looked like it followed best practices without understanding why those practices exist. Design patterns were applied in contexts where they added complexity without benefit. Abstractions were introduced where simple code would suffice.

Developers who resisted AI assistance based on these concerns were often making accurate observations. The tools were not ready for production-critical work without substantial human oversight.

### 4.2 What Has Changed (Late 2025-2026)

The tools have improved substantially in the intervening period:

- **Hallucination has decreased markedly.** Current models, trained on larger and more current codebases, rarely invent nonexistent APIs. When they suggest deprecated approaches, they more frequently note the deprecation and offer alternatives. The burden of verifying every line has reduced.
- **Edge case handling has improved.** Models with larger context windows and better instruction-following generate more robust code by default. Null checks, error handling, and boundary conditions appear more reliably—not perfectly, but meaningfully better.
- **Security awareness has increased.** Current models more consistently avoid obvious anti-patterns. They suggest parameterized queries rather than string concatenation for database access, escape output by default, and flag uncertainty about security implications more reliably.
- **Contextual pattern application.** Given sufficient codebase context—via tools that index repositories—suggestions increasingly respect existing patterns, naming conventions, and architectural decisions rather than cargo-culting generic approaches.

This does not mean the tools are now perfect. It means that specific concerns from 2023 may no longer apply in 2026.

### 4.3 Concerns That Remain Legitimate

Some concerns remain valid regardless of tool improvement:

- **Domain Knowledge.** AI does not understand your business. It can generate technically correct code that is functionally wrong because it does not know that "account balance" in your system has specific semantics, or that certain operations must occur in a particular order for regulatory compliance.
- **Novel Problem-Solving.** For well-established patterns and common tasks, current AI is highly capable. For genuinely novel architectural challenges—the problems that distinguish senior engineers—AI serves as a thought partner at best, not a solution source.
- **Long-Range Coherence.** AI can maintain consistency within a file or module. Ensuring coherence across a large system with complex interdependencies—understanding how changes in one component affect others—still requires human architectural judgment.
- **Licensing and Intellectual Property.** Models trained on open-source code may reproduce substantial portions, potentially introducing licensing obligations. This remains a genuine concern in commercial contexts.

### 4.4 The Mental Model Gap

Here is the critical observation: **AI coding tools are improving faster than mental models are updating.**

A developer who evaluated Copilot in 2022 or ChatGPT code generation in early 2023 formed a mental model based on that experience. That model was accurate then. The tools have since improved through multiple generations. Without deliberate re-evaluation, professionals carry outdated assessments into a changed reality.

This creates a specific category of resistance: concerns that were legitimate when formed but are no longer proportionate to current capabilities. The person is not wrong about their past experience; they are wrong about current tools.

The practical diagnostic:
- Is the concern based on recent, direct experience with current tools?
- Is the concern specific and testable?
- Is the person willing to try current tools in a low-stakes context?

If the answers are no, no, and no, the resistance may be stale rather than rational.

The appropriate intervention is not argument but invitation: "The tools have changed substantially. Would you be willing to try current-generation assistance on a non-critical task and form a fresh assessment?"

---

## 5. The Augmentation Asymmetry

I've been contemplating a question: *why does cognitive augmentation provoke anxiety that physical augmentation does not?*

A crane operator lifts loads no human could ever lift unaided. We don't worry about "false confidence" in their lifting ability. We don't suggest they must understand hydraulics and metallurgy to operate the machine responsibly. We accept that human capability is extended by machinery and celebrate the results.

When AI enables a developer to produce code beyond what they could write unaided, the framing shifts. We worry about Dunning-Kruger effects, about perceived competence exceeding actual competence, about developers who cannot debug what they did not understand. The gap between capability and understanding is treated as a risk rather than an achievement.

Several factors may explain this asymmetry:

- **Verification difficulty.** Physical output is immediately verifiable. The crane either lifted the load or it did not. Code verification is harder—a function can appear to work while containing subtle bugs, security vulnerabilities, or architectural flaws that will not manifest until production, under load, at an inconvenient hour.
- **Maintenance coupling.** When a crane breaks, the operator calls a mechanic. The roles of operator and repairer are separate. Software has traditionally coupled production and maintenance: the developer who writes code is expected to debug it when problems emerge. If they do not understand what they wrote, they cannot maintain it.

  But is this coupling inherent or merely historical? One can imagine AI-assisted production paired with AI-assisted debugging, where human judgment focuses on requirements and verification rather than implementation details.
- **Learning trajectories.** We don't expect crane operators to eventually lift heavy loads themselves. But we expect junior developers to become senior developers, and conventional wisdom holds that this requires struggle—working through hard problems to build intuition. If AI allows skipping that struggle, will judgment develop?

  Perhaps. Or perhaps judgment develops differently when the work is different. The skills required for AI-augmented development may not match the skills developed through traditional apprenticeship.
- **Identity attachment.** We don't identify as a "lifter." Physical limitations do not threaten professional identity because we have long accepted that machines extend physical capability. But "thinker" and "problem-solver" are core to how many developers understand themselves. Cognitive augmentation touches identity in ways physical augmentation does not.

The question this raises: How much of our concern about AI-assisted development is practical (verification difficulty, maintenance coupling, learning trajectories) and how much is existential (discomfort with machines performing cognitive work we thought was distinctively human)?

The practical concerns can be addressed with appropriate processes. The existential concerns require something different: evolving our understanding of what it means to be competent in an augmented world.

---

# Part III: Types of Resistance

## 6. Three Kinds of No

The historical cases suggest that resistance is not one thing. The Viennese doctors rejecting Semmelweis, the surgeons wary of early anesthesia, and the Luddites smashing looms were all saying "no" — but to different things, for different reasons, with different prospects of coming around. Conflating them is a mistake leaders make repeatedly, and it leads to interventions that are precisely wrong.

### 6.1 Rational Caution

Consider a surgeon in 1848 who declines to use ether on a patient with a heart condition. Early anesthesia was genuinely dangerous; mortality from overdose and cardiac arrest was documented. This surgeon has examined the evidence, judged the risk too high for this patient, and made a defensible clinical decision. Show her improved techniques next year and she may well adopt them.

The software equivalent is familiar. A developer says: "The AI doesn't understand our business domain — its suggestions are technically correct but functionally wrong for our context." Or: "Our regulatory environment requires human review of all code changes, and AI assistance doesn't change that requirement." These concerns are grounded in recent, direct experience with the tools as they actually exist. They point to specific limitations. They are, crucially, open to evidence — the person holding them would adopt AI assistance in contexts where those limitations do not apply.

The distinguishing quality of rational caution is that it is testable. It makes predictions that can be checked against reality. And it does not need to be overcome so much as respected. Some concerns — domain knowledge, regulatory constraints, the need to reason through genuinely novel problems — will remain valid regardless of how much the tools improve. The appropriate response is guardrails, not persuasion.

### 6.2 Stale Caution

Now consider a different surgeon, one who tried ether in 1847, lost a patient to overdose, and has refused to touch it since. It is 1870. Anesthesia techniques have advanced enormously. The mortality that haunted his early experience has been addressed by better dosing, better monitoring, better training. But his mental model has not updated. He is refusing a technology that no longer exists.

His caution was rational when it formed. The problem is that it calcified. The experience was real; the conclusion drawn from it has outlived its evidence.

This pattern is everywhere in the current debate about AI-assisted development. "AI generates code full of security vulnerabilities" — accurate for 2023-era tools, substantially less true in 2026. "You have to verify every single line, it hallucinates APIs constantly" — dramatically reduced with current models. "The code it produces is unmaintainable" — no longer characteristic when tools have full codebase context.

The distinguishing feature is the date stamp on the experience. Twelve to twenty-four months old, perhaps. The person may not have touched current-generation tools at all. And unlike identity-based resistance, stale caution is often amenable to fresh evidence — but only first-hand evidence. Arguing about it accomplishes nothing, because the person's past experience was valid. What works is invitation: "The tools have changed substantially. Would you try current-generation assistance on a low-stakes task and form your own assessment?" Experiences from respected peers who have updated their views carry more weight than any argument from management.

### 6.3 Identity-Based Resistance

Then there is Charles Meigs, the American obstetrician who declared that a gentleman's hands could not transmit disease. Meigs was not responding to evidence about hand-washing — he was responding to what hand-washing *implied*. If Semmelweis was right, then doctors had been killing their patients. Not through malice or incompetence, but through the very practices that defined their profession. The psychological cost of that recognition was unbearable.

When a developer says "real developers don't use AI assistance," or "if AI can do this, what's the point of my expertise?" — they are not making an empirical claim about tool capability. They are expressing something about meaning and self-concept. The resistance sounds categorical and absolute rather than specific and contextual. "AI-generated code can never be as good as hand-crafted code" — stated as a principle, regardless of evidence. And when one objection is addressed, new ones emerge, because the underlying issue has not been resolved.

This is not irrational. It is deeply human. The master weavers who smashed looms were not making a technical assessment of cloth quality; they were defending an understanding of what made them valuable. The scribes who opposed printing were defending not employment but a way of life organised around the sacred act of copying texts.

Evidence does not resolve identity-based resistance, because evidence was never the issue. What helps — slowly, over time — is the development of new professional narratives. "Your expertise is what allows you to recognise when AI is wrong." "The craft evolves; it doesn't disappear." And above all, seeing respected peers who have found ways to accommodate new tools without surrendering what makes their work meaningful.

### 6.4 Why the Distinction Matters

The three types require entirely different responses, and applying the wrong one makes things worse. Arguing with rational caution alienates thoughtful people. Respecting stale caution as though it were current leaves teams stuck in the past. And presenting evidence to someone in the grip of identity threat only intensifies the resistance — it feels like an attack on who they are.

The questions that help distinguish them are simple enough:

| | Rational Caution | Stale Caution | Identity Resistance |
| --- | --- | --- | --- |
| Based on recent experience? | Yes | No — often 12-24 months old | Not relevant |
| Specific and testable? | Yes | Yes, but outdated | Categorical |
| Open to new evidence? | Yes | Yes, if first-hand | Reluctant |
| Willing to try current tools? | Yes, with guardrails | Often yes | Often no |

But no table captures the full picture. Reading resistance well is more like reading people — it requires attention to what is said, what is meant, and what is being protected.

---

## 7. The Problem of Pace

There is a complication that the historical cases illuminate but do not fully parallel: the speed at which AI coding tools are improving, and the challenge this creates for anyone trying to maintain an accurate assessment.

The closest analogy may be anesthesia between 1846 and 1880. The technology moved from genuinely dangerous to broadly safe within a single career span. A surgeon who formed her assessment in 1847 and did not revisit it in 1870 was applying caution to a technology that had fundamentally changed beneath her. The danger she remembered had largely receded; her mental model had not.

But even this understates the current problem. Anesthesia improved over decades. AI coding tools are improving over months. The gap between GPT-3.5's code generation in early 2023 and the capabilities available in early 2026 is not incremental — it spans multiple generations of architecture, training methodology, and tooling integration. A developer who evaluated Copilot in its first year and found it wanting may now be resisting a tool that bears only superficial resemblance to what they tried.

This creates a category of resistance that does not map cleanly onto any of the three types described above. It looks like rational caution — it is grounded in real experience, points to specific concerns, and sounds entirely reasonable. But the experience is stale, and the person may not know it. They have not actively refused to re-evaluate; they simply have not had occasion to. The assessment calcified not through stubbornness but through the ordinary human tendency to treat past experience as current.

The pace also explains something puzzling about the adoption curve. In most historical cases, expert resistance softened gradually as the technology proved itself over years or decades. The Luddite movement lasted five years. Resistance to anesthesia took roughly a generation to fade. Scribes and printers coexisted for decades before the transition completed.

AI-assisted development does not have that kind of time. The tools that existed eighteen months ago are already substantially obsolete. The tools available today will likely be superseded within a year. Professionals who wait for the technology to "settle down" before forming an opinion may find that settling down is not what this technology does.

Those who successfully navigated the anesthesia transition shared certain characteristics: ongoing exposure to improved techniques rather than reliance on memories of early failures, observation of colleagues using the technology successfully, and a willingness to test assumptions against current evidence rather than past experience. The same characteristics distinguish developers who are navigating the current transition well.

The categorical error of the anesthesia era — "this will never be safe enough" — confused current limitations with permanent ones. The analogous error today — "AI assistance will never be good enough for production code" — may be making the same mistake. But it is a harder mistake to avoid when the ground is shifting this fast.

---

# Part IV: The Transformation of Practice

## 8. Individual Skills, Team Capability, and the Work Itself

The preceding sections examined resistance and adoption at the level of individual psychology and historical pattern. What follows addresses something larger: the fundamental transformation of how software development happens.

A crucial premise underlies everything that follows: **value is delivered through teams of people working together toward agreed outcomes.** Individual skill matters because it contributes to team capability. Team capability matters because it enables value delivery. Neither exists in isolation.

This is not a philosophical abstraction. It has immediate practical implications for how anyone should think about AI adoption. An organisation cannot simply develop individual skills and hope teams figure themselves out. Nor can it build team infrastructure and assume individuals will grow into it. The individual and collective dimensions must evolve together.

Consider a rugby team. Individual skills matter enormously — passing accuracy, tackling technique, positional awareness, fitness. But a team of individually brilliant players who cannot coordinate will lose to a team of competent players who work together effectively. The scrum requires eight individuals to bind and push as one. The lineout requires precise timing between thrower, jumper, and lifters. The backline move requires each player to run the right line at the right time.

At the same time, no amount of coordination can compensate for players who lack basic skills. A team with perfect plays but players who cannot catch will fail. Individual capability is the foundation; team coordination amplifies it.

The same dynamic applies to AI-assisted software development. Individual practitioners must develop new skills — prompting effectively, evaluating AI output, operating at higher stages. But they must also coordinate: sharing context, avoiding duplication, maintaining architectural coherence, managing costs collectively. Neither dimension alone is sufficient.

Three interconnected transformations are worth examining:
1. **The transformation of practice itself**—what the work is becoming
2. **Individual skill evolution**—the eight stages of practitioner development
3. **Team capability evolution**—the five levels of organizational maturity

These are not separate topics but three views of the same phenomenon.

---

## 9. The Transformation of Practice

Before examining how individuals and teams evolve through this transition, it is worth pausing to consider what the work itself is becoming. This is not merely "coding with AI assistance" in the way that a carpenter might use a power saw instead of a hand saw — doing the same work faster. The nature of the activity is changing.

### 9.1 From Composition to Direction

Traditional software development is fundamentally compositional. The developer thinks through a problem, designs a solution, and writes code to implement it — line by line, function by function, the artifact emerging from the developer's mind through their hands. It is, in this sense, a craft of making. The developer is author and composer.

At higher stages of AI-assisted development, the work shifts from composition to direction. The developer specifies what should be built, evaluates what the AI produces, provides course corrections, and verifies outcomes. The code still emerges, but the developer's relationship to it has changed. They are less author than editor, less composer than conductor.

The parallel to surgery is instructive. Before anesthesia, the surgeon's hands were the instrument — every cut deliberate, every motion trained over years of practice. When anesthesia arrived and, later, when teams and technology expanded the operating theatre, the senior surgeon's role shifted from sole performer to director of an ensemble: guiding junior surgeons, coordinating with anesthesiologists, interpreting diagnostic information. The manual skill did not become irrelevant, but it was joined — and eventually subordinated — by the skill of orchestration.

### 9.2 From Serial to Parallel

Traditional development is largely serial. A developer works on one problem, completes it or reaches a stopping point, then moves to the next. Deep focus on a single problem is valued. Flow states — extended periods of uninterrupted concentration — are prized and protected by teams that understand their importance.

At higher stages, work becomes parallel. Multiple AI agents work simultaneously on different tasks. The practitioner tracks multiple work streams, context-switches between them, triages across competing demands. The rhythm changes: shorter attention on each thread, broader attention across many threads.

The weaver at a single loom knew every thread intimately. The factory foreman overseeing fifty looms knew none of them that way — but understood something the weaver did not: how the whole operation fitted together, where bottlenecks formed, which machines needed attention. It was a different kind of expertise, not lesser but genuinely different. Some weavers became excellent foremen. Others never adapted to the breadth of attention the new role demanded.

### 9.3 From Understanding to Verification

Traditional development emphasises deep understanding. A developer is expected to comprehend every line of code they produce, to explain why each decision was made, to debug any problem that emerges. "I don't understand how this works" is an uncomfortable admission for any professional, but especially for one whose professional identity rests on understanding complex systems.

At higher stages, understanding gives way to verification. The practitioner may never read most of the code produced by their agents. What matters is whether it works: does it pass tests? Does it meet acceptance criteria? Does it integrate correctly? Understanding the implementation becomes optional if the outcomes are verified.

Trithemius, the abbot who defended scribes against the printing press, would have recognised this anxiety. The scribe understood every word he copied — he was scholar as much as copyist, catching errors, annotating margins, bringing knowledge of textual tradition to every page. The publisher who verified printed output against a manuscript was performing a different act: checking correspondence, not understanding content. It was more efficient and less intimate. Something was gained in reach; something was lost in depth.

### 9.4 From Maker to Evaluator

Perhaps the most consequential shift is from creation to assessment. Traditional development is creative work. The developer makes things — designs, implements, brings something into existence that was not there before. This act of creation is often a source of deep satisfaction: the flow state of writing clean code, the elegance of a well-designed abstraction, the quiet pleasure of a program that runs correctly on the first attempt.

At higher stages, the work shifts toward evaluation. The practitioner prompts, waits, reads output, judges correctness, identifies gaps, provides feedback, iterates. The primary cognitive activity is assessment rather than creation. Psychological research distinguishes between generative and evaluative cognitive modes. Generative work — making something — tends to produce energy and engagement. Evaluative work — judging something — tends to produce decision fatigue. The shift from maker to evaluator changes not just what the work is but how it feels.

The British Musicians' Union sensed something similar in 1984 when they tried to ban synthesisers. A session musician who had spent decades mastering the oboe was not merely losing work to a keyboard player pressing buttons. He was losing the experience of playing — the breath control, the embouchure, the physical relationship with the instrument that was inseparable from the music itself. The synthesiser player produced acceptable sound. The oboist had produced it through his body. Whether the audience noticed the difference was, in a sense, beside the point.

### 9.5 What This Means

These four shifts — composition to direction, serial to parallel, understanding to verification, making to evaluating — do not merely augment existing practice. They constitute a transformation of it. And this has implications that run deeper than workflow or productivity.

The skills that made someone excellent at traditional development do not automatically transfer. A brilliant composer is not necessarily a brilliant conductor. Selection criteria may change: the traits that predicted success in the old mode — deep focus, perfectionism, the ability to hold complex systems in working memory — may matter less than traits that were previously secondary: rapid context-switching, tolerance for ambiguity, the ability to evaluate quality without having created it.

Some practitioners will not thrive in the transformed practice. This is not a reflection of intelligence or effort; it is a question of fit. The 45-year-old master weaver did not lack skill. He possessed the wrong skills for a world that had changed around him.

---

## 10. The Eight Stages

Steve Yegge — a veteran of thirteen years at Google and six at Amazon, known for long-form essays that the industry treats as manifestos — published ["Welcome to Gas Town"](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04) in January 2026. The essay is characteristic Yegge: sprawling, opinionated, and uncomfortably prescient. Its central claim is that development has shifted from typing to orchestration, and that most of the industry has not noticed.

Yegge describes a progression through eight stages of individual AI adoption, each representing a qualitative shift in how the practitioner relates to their tools and their code. The metaphor that runs through his essay is a frontier settlement — a "Gas Town" where fleets of autonomous agents do the labour while the human acts as mayor or sheriff. The progression is not merely from "less AI" to "more AI." It is from author to orchestrator, from making to governing.

What follows draws on Yegge's framework, adapted and contextualised for the broader argument of this essay.

### 10.1 The Early Stages: Assistance

The first three stages represent what most developers currently experience. At **Stage 1**, AI is a reference tool — occasional code completion, the odd question to a chatbot. The developer writes all code themselves. The experience is not fundamentally different from using Stack Overflow or an API reference.

At **Stage 2**, a coding agent operates in the IDE sidebar, requesting permission before each action. The developer reviews and approves every step. It is, in Yegge's framing, the "sandbox" phase — the agent is kept on a tight leash. Trust is low; oversight is high. Most enterprises that have formally adopted AI-assisted development have arrived here. Many have stopped.

**Stage 3** is what Yegge calls "YOLO mode" — permissions are switched off, and the agent executes without pre-approval. This is a psychological threshold more than a technical one. The developer has accumulated enough experience to trust the agent's judgment in routine cases and intervene only when problems emerge. It is analogous to the moment a surgeon first allows an anesthesiologist to manage the patient without being asked to approve each dose.

### 10.2 The Middle Stages: Transformation

Stages 4 and 5 are where the nature of the work begins to change. At **Stage 4**, the agent's output becomes the primary view. The developer stops looking at code being written and begins looking at diffs — reviewing what was produced rather than producing it. Yegge describes the agent "filling the screen": the code is there, but the developer's relationship to it has shifted from author to editor.

At **Stage 5**, work moves from the IDE to the command line. The agent operates with full autonomy across the codebase. The developer's screen shows diffs scrolling past — some examined carefully, some glanced at, some not reviewed at all. The IDE, for decades the centre of the developer's working life, becomes a secondary tool.

These middle stages are where the identity questions begin to bite. A developer at Stage 2 is still writing code, with AI helping. A developer at Stage 5 is directing agents and evaluating output. The work is recognisably different. It is at this transition that the resistance patterns described in Part III most often surface — not at the entry point where AI is merely a convenience, but at the inflection point where it transforms the role.

### 10.3 The Later Stages: Orchestration

**Stage 6** introduces parallelism: three to five agent instances running concurrently on different tasks. Throughput increases dramatically. The developer's role becomes coordination — tracking multiple work streams, resolving conflicts between them, deciding which output to review carefully and which to accept on trust.

**Stage 7** pushes this further: ten or more agents simultaneously, at the limits of what a human can track manually. The cognitive load is no longer about the problems being solved but about the logistics of managing the agents solving them. Merge conflicts multiply. Context-switching becomes the primary activity.

And then there is **Stage 8** — what Yegge calls "Gas Town" itself. The practitioner builds or uses orchestration systems that coordinate the agents automatically: automated handoffs, merge queues, supervision agents that monitor other agents. Yegge describes merging forty-four thousand lines of code that no human reviewed, managed overnight by what he calls "PR Sheriffs." The practitioner at this stage does not manage code or even manage agents. They manage the system that manages the agents that produce the code. It is, as Yegge puts it, a refinery — and the developer is running it.

### 10.4 The Uneven Journey

The temptation is to read these stages as a ladder: higher is better, and everyone should climb. That reading is mistaken in several ways.

First, different work suits different stages. A genuinely novel architectural challenge — the kind of problem that distinguishes senior engineers — may remain Stage 3 or 4 work even for a practitioner who routinely operates at Stage 7 elsewhere. The stages are not a measure of the person but of the activity.

Second, temperament matters. Some practitioners thrive in the parallel, evaluative mode of higher stages — the rapid context-switching, the breadth of attention, the tolerance for imperfect output. Others find deep satisfaction in the compositional, focused mode of traditional work. The developer who enters flow state while crafting an elegant algorithm is not inferior to the developer managing fifteen agents. They are suited to different work.

Third, infrastructure constrains progression. A practitioner cannot sustainably operate at Stage 6 or above without organisational support: cost management, merge coordination, governance. The individual and the team must evolve together — which is the subject of the next section.

---

## 11. The Five Levels

Yegge's framework describes how individuals progress. But individuals do not work alone, and organisational infrastructure either enables or constrains what individuals can do. Siddhant Khare — a software engineer at Ona and core maintainer of OpenFGA, the CNCF's fine-grained authorisation project — addresses this gap in [*The Agentic Engineering Guide*](https://agents.siddhantkhare.com/), published in early 2026. His ["Agent Maturity Model"](https://agents.siddhantkhare.com/22-agent-maturity-model/) describes not individual skill but collective capability: the infrastructure, policies, and coordination mechanisms that determine what AI-assisted work an organisation can sustain.

Khare's central insight is disarmingly simple: **maturity is defined by your weakest dimension, not your most advanced capability.** A team with sophisticated agent orchestration but no security policies is not a mature team. They are a liability. The model is not a ladder to be climbed but a set of interlocking capabilities that must develop together.

### 11.1 From Shadow IT to Infrastructure

The first two levels describe what most organisations experience today. At **Level 1 — Experimentation** — individual engineers try AI tools on their own initiative. There are no team standards, no security policies, no cost tracking. Usage is invisible to the organisation. It is, in effect, shadow IT — and like all shadow IT, it carries risks that compound the longer it goes unacknowledged.

**Level 2 — Individual Adoption** — represents the point where AI use becomes acknowledged but not managed. Engineers use AI tools regularly for personal productivity. Some informal knowledge sharing occurs — tips in Slack channels, the occasional demo at a team meeting. But there are no team-wide standards, no formal policies, no systematic approach. Each developer's relationship with AI is personal and idiosyncratic.

The transition from Level 1 to Level 2 is straightforward: provide licensed tools, create channels for sharing, track usage without mandating it. Most organisations that have "adopted AI" have reached this point and declared victory. Khare argues they have barely begun.

### 11.2 The Infrastructure Threshold

**Level 3 — Team Integration** — is where the character of the transformation changes. At this level, the organisation begins building infrastructure specifically for AI-assisted work: context files in repositories that give agents project-specific guidance, internal APIs exposed through tool servers so agents can interact with proprietary systems, review guidelines for AI-assisted code, and cost tracking to make the economics visible.

This is the level at which AI tools stop being individual productivity aids and begin functioning as team infrastructure. The difference is significant. A developer using AI at Level 2 is faster individually but adds nothing to team capability. At Level 3, the investment compounds: every context file benefits every team member, every integrated API makes every agent more capable, every review guideline reduces coordination overhead.

The historical parallel is suggestive. When spreadsheets first appeared, individual accountants used them to speed up their own calculations — but the transformation of accounting happened when firms standardised templates, built shared models, and created review processes for spreadsheet work. The tool became infrastructure. The same transition is happening now.

### 11.3 Orchestration and Autonomy

**Level 4 — Orchestration** — deploys multi-agent workflows with formal governance: authorisation systems controlling what agents can access, observability and tracing to make agent activity visible, documented policies, cost budgets with alerts. This is the level that enables individual practitioners to operate at Stages 5 through 7. Without it, advanced individual practice is unsustainable — brilliant cowboys riding without fences.

**Level 5 — Autonomy** — is where agents operate without direct supervision. Background agents run overnight, monitored by anomaly detection rather than human attention. Review shifts from inspecting actions — how did the agent do it? — to evaluating outcomes: did the problem get solved without regressions? Agents function as team members in the way that CI/CD pipelines function as part of deployment — always running, occasionally needing attention, but not requiring a human in the loop for routine operation.

Few organisations have reached Level 5. It requires not just technical infrastructure but a fundamental shift in how the organisation thinks about trust, accountability, and verification. It is, in Khare's framing, the point at which the organisation has genuinely transformed — not merely adopted tools but changed how it works.

### 11.4 The Maturity Trap

Khare identifies a pattern he calls the "maturity trap": organisations that try to skip levels, jumping from Level 2 individual use to Level 4 orchestration without building the Level 3 infrastructure of context files, integrated APIs, and review guidelines. The result is brittle and unmanageable — sophisticated agent workflows running against codebases the agents do not understand, producing output that no one has established standards for reviewing.

The temptation to skip is understandable. Level 3 work — writing context files, integrating internal APIs, establishing review guidelines — is unglamorous infrastructure. Level 4 work — multi-agent orchestration, automated workflows — is exciting and visible. But the excitement is short-lived when the orchestrated agents lack the context to produce useful work.

Attempting to skip levels recalls the textile manufacturers who bought power looms before establishing the factory systems to support them. The machinery was impressive. Without maintenance schedules, quality inspection, and coordination of workers, it was also unreliable and dangerous. Infrastructure precedes capability, in software as in manufacturing.

---

## 12. The Interdependence

The individual and team dimensions described in the previous two sections are not independent tracks to be managed separately. They are aspects of a single transformation, and they must evolve together.

### 12.1 The Rugby Analogy

Consider a rugby team. Individual skills matter enormously — passing accuracy, tackling technique, positional awareness, fitness. But a team of individually brilliant players who cannot coordinate will lose to a team of competent players who work together effectively. The scrum requires eight individuals to bind and push as one. The lineout requires precise timing between thrower, jumper, and lifters. The backline move requires each player to run the right line at the right moment.

At the same time, no amount of coordination can compensate for players who lack fundamental skills. A team with perfect set-piece plays but players who cannot catch will fail. Individual capability is the foundation; team coordination amplifies it.

The same dynamic applies to AI-assisted development. Individual practitioners must develop the skills described in Yegge's stages — prompting effectively, evaluating output, operating in parallel, orchestrating agents. But they must also coordinate: sharing context, avoiding duplication, maintaining architectural coherence, managing costs collectively. Neither dimension alone is sufficient.

### 12.2 When the Dimensions Diverge

When individual skill and team infrastructure evolve out of alignment, the results are predictable and painful.

The first failure mode is high individual capability within low team maturity — what amounts to shadow IT. Skilled practitioners operating at Stage 6 or 7 within a Level 1 or 2 organisation. They are productive, sometimes spectacularly so, but their work is ungoverned. Costs are invisible to the organisation. Security risks accumulate as agents access systems without formal authorisation. There is no observability into what the agents are doing. When something goes wrong — and eventually it does — the organisation has no infrastructure to detect, diagnose, or remediate the problem.

This is the software equivalent of having individually brilliant rugby players who run their own plays regardless of what the team is doing. Occasionally spectacular. Frequently chaotic. Ultimately unsustainable.

The second failure mode is the reverse: sophisticated team infrastructure with practitioners stuck at early stages. A Level 4 organisation whose developers are still at Stage 2. The infrastructure exists — orchestration systems, authorisation frameworks, observability tools — but nobody uses it. The investment sits idle, producing no value, generating frustration from management that expected transformation and from practitioners who feel pressured to use tools they have not learned.

This is the team with an elaborate playbook and players who cannot execute it. The theory is sophisticated; the practice is basic.

### 12.3 Developing Together

Effective transformation develops both dimensions in concert. Team infrastructure should lead individual capability by roughly one level, ensuring support exists when practitioners are ready to progress. Pioneers — those who advance faster — should be supported without requiring everyone to match their pace. And progression should be pulled by demonstrated value rather than pushed by management enthusiasm.

The historical cases offer a consistent lesson here. The organisations that navigated transitions well — the publishers who grew from printing houses, the accounting firms that embraced spreadsheets, the recording studios that integrated synthesisers — all developed individual skill and collective infrastructure together. The publishers who hired former scribes got both the craft knowledge and the new technology. The accounting firms that trained senior partners alongside junior analysts avoided the generational rift that paralysed less thoughtful firms. The studios that kept session musicians on staff while adding synthesiser capabilities preserved what was valuable while adopting what was new.

The team is the unit that delivers value. Individual skill matters because it contributes to team capability. A Stage 7 practitioner in isolation produces impressive individual output. A team where multiple members operate at Stage 5 or 6, supported by Level 4 infrastructure, delivers coordinated value at scale. The transformation is not about creating AI-augmented individuals. It is about creating AI-augmented teams.

---

## 13. The Human Cost

The transformation of practice creates genuine difficulties that deserve honest acknowledgment rather than corporate optimism. Siddhant Khare's essay ["AI Fatigue Is Real"](https://siddhantkhare.com/writing/ai-fatigue-is-real), which reached the top of Hacker News in February 2026, struck a nerve because it named something many practitioners were feeling but few had articulated: the paradox that AI makes individual tasks faster while making days harder.

### 13.1 The Productivity Paradox

The explanation is straightforward. When each task takes less time, more tasks fill the day. Capacity appears to expand, so expectations expand to match. A developer who once spent a full day on one design problem might now touch six problems in a day, each "only taking an hour with AI."

But context-switching between six problems is cognitively expensive. The AI does not tire between problems. The human does. AI reduces the cost of production while increasing the cost of coordination, review, and decision-making. Those costs fall entirely on the human.

### 13.2 The Loss of the Meditative Middle

Khare identifies something subtler that resonates with many practitioners: the loss of what he calls the "meditative middle." Traditional coding has three cognitive modes — stressful problem-solving at the outset, meditative implementation in the middle, and the satisfaction of completion at the end. The middle phase, where the developer translates an understood solution into code, is not merely productive. It is restorative. The fingers move; the mind settles; the work has a rhythm.

AI collapses this middle phase. The hard decisions remain — what to build, how to structure it, what trade-offs to accept. And the evaluation remains — reading output, judging correctness, catching errors. But the meditative implementation, the part where the developer could briefly rest their higher cognition while the hands did familiar work, is gone. The result is a working day compressed into an unbroken sequence of cognitively demanding tasks: decision, evaluation, decision, evaluation, with no respite between them.

Engineers describe this as being "tired but not sure why" — a state Khare attributes to executive functioning fatigue. The prefrontal cortex, which handles judgment and decision-making, runs at full load all day. The breaks it used to get during routine implementation have been automated away.

### 13.3 From Maker to Evaluator

Before AI assistance, a developer's day had a recognisable shape: think about a problem, write code, test it, ship it. The developer was a creator. With heavy AI assistance, the role increasingly becomes: prompt, wait, read output, evaluate correctness, evaluate safety, evaluate architectural fit, fix inadequacies, re-prompt, repeat. The developer becomes an evaluator — a quality inspector on an assembly line that runs at whatever speed the AI can produce.

This is a fundamentally different kind of work. Creating generates flow states and energy. Evaluating generates decision fatigue and depletion.

A further complication: AI-generated code often requires more careful review than a colleague's work. When reviewing a colleague's pull request, the reviewer knows their patterns, strengths, and blind spots. With AI, every line is potentially suspect. The reviewer cannot rely on the author's reputation or track record, because the author has neither.

### 13.4 Nondeterminism

Engineers are trained on determinism. Same input, same output. This principle makes debugging possible, makes systems reasonable, makes professional confidence well-founded. AI breaks this contract. A prompt that worked Monday produces different results Tuesday. There is no stack trace explaining why the model chose differently.

For practitioners whose careers are built on "if it broke, I can figure out why," this nondeterminism creates a persistent low-level discomfort. You cannot fully trust output. You cannot fully predict behaviour. You cannot fully relax. The professional identity built on understanding systems encounters a system that resists being understood.

### 13.5 The Skills Mismatch

The traits that made someone excellent at traditional development may work against them in AI-augmented work. Deep focus on a single problem — the ability to hold a complex system in working memory for hours — becomes less valuable when the work demands rapid switching between multiple agent streams. Perfectionism, which once produced elegant code, becomes a liability when "good enough" output arrives in minutes and perfecting it takes hours. Deterministic thinking, the hallmark of engineering rigour, encounters probabilistic output that cannot be reasoned about with the same tools.

This is not a judgment about which traits are superior. It is a statement about fit. The role has changed, and some people's characteristics suit the new role better than others. The master weavers of 1811 did not lack skill or intelligence. They possessed skills and intelligence superbly adapted to a world that was disappearing beneath their feet.

### 13.6 What This Means for Teams

These human costs have collective implications that thoughtful leaders cannot ignore. Not everyone will progress to Stage 6 or beyond, and not everyone should. Stage 3 or 4 might be the appropriate ceiling for excellent practitioners whose temperament does not suit the evaluative, parallel mode of higher stages — and that ceiling should be respected, not treated as a failure.

Teams should watch for fatigue. The practitioner shipping more than ever while appearing increasingly drained is a warning sign that productivity metrics will not capture. If teams still need deep creative problem-solving — and they do — some practitioners should be protected from the evaluation-heavy workflow.

And there is a harder truth: if the practice has genuinely transformed and someone's characteristics do not suit it, honest acknowledgment is more humane than pretending everyone can adapt. The Luddite weavers deserved honesty about their prospects. So do practitioners who find themselves misaligned with the transformed role.

---

# Part V: Navigating the Transition

## 14. A Framework for Change

How should an organisation navigate a transformation that affects individual skill, team infrastructure, professional identity, and the nature of the work itself? The historical cases suggest that the transition will happen regardless of how it is managed — the question is whether it happens well or badly, and how much unnecessary damage accumulates along the way.

### 14.1 Principles

Several principles emerge from the patterns examined throughout this essay.

The first is that **value flows through teams.** The goal is not to create AI-augmented individuals but AI-augmented teams that deliver outcomes. Yegge's eight stages describe individual progression; Khare's five levels describe team infrastructure. Neither dimension alone produces value. An organisation of Stage 7 individuals in a Level 2 environment is a collection of brilliant cowboys. A Level 5 environment staffed by Stage 2 practitioners is an empty cathedral. The dimensions must evolve together, with team infrastructure leading individual progression by roughly one level.

The second is that **resistance is not one thing**, and treating it as such wastes effort while alienating the people most worth keeping. Rational caution deserves guardrails. Stale caution deserves fresh experience. Identity-based resistance deserves acknowledgment and time. Applying the wrong intervention — arguing with rational caution, respecting stale caution as current, presenting evidence to someone in identity crisis — makes things worse.

The third is that **the transformation has genuine costs**, and pretending otherwise destroys trust. AI fatigue is real. The shift from maker to evaluator does not suit everyone. Some excellent developers will not thrive in the transformed role. Leaders who acknowledge these costs honestly earn the credibility to lead through them. Leaders who promise that "nothing fundamental is changing" lose credibility the moment practitioners discover otherwise.

The fourth is that **augmentation, not replacement, must be the lived experience**, not merely the talking point. If practitioners experience AI adoption as a headcount reduction tool, resistance becomes rational regardless of what leadership says. The constraint in most organisations is not excess people but excess work. AI should be directed at the constraint.

### 14.2 What Leaders Get Wrong

The most common failure is treating adoption as a procurement problem: buy tools, roll them out, measure adoption rates, declare success. This misses everything that matters.

Leaders who say "we need to improve productivity by X per cent" signal that the point is doing more with fewer people — and every practitioner hears the subtext. Leaders who say "this is the future and everyone needs to get on board" dismiss legitimate concerns and trigger identity resistance. Leaders who point to competitors — "other companies are adopting rapidly" — create pressure without addressing substance.

What works is different. "We are exploring how AI can remove tedious work and let you focus on harder problems" — this frames adoption as augmentation, not displacement. "Your expertise is essential for guiding AI output and catching its limitations" — this positions the practitioner's existing skill as the safety net, not the thing being replaced. "We will measure success by quality and experience, not just velocity" — this signals that the organisation values what practitioners value.

And when the costs are real — the fatigue, the identity disruption, the discomfort of nondeterminism — leaders must name them: "AI can change work in ways that are tiring for some people. That is real. If you are producing more but feeling more depleted, that experience is valid. Different work styles suit different people. There is no shame in finding your appropriate level."

### 14.3 Common Objections and Honest Responses

Certain objections recur in every organisation navigating this transition, and they deserve thoughtful engagement rather than dismissal.

"We are moving too slowly" is often heard from enthusiasts and management alike. Speed of adoption is not equivalent to quality of adoption, and mandating rapid uptake without addressing concerns creates technical debt, resentment, and vulnerabilities. But organisations should honestly assess their position. If competitors operate at higher stages and levels, "sustainable pace" may be a polite name for falling behind.

"The sceptics are simply resistant to change" flatters leadership's patience while mischaracterising the situation. Some sceptics may indeed be resistant in a categorical sense. Many are not. The burden is on leadership to distinguish rational caution from stale caution from identity-based resistance, and to address each appropriately. Labelling all scepticism as resistance is the organisational equivalent of the Viennese medical establishment dismissing Semmelweis: it protects the ego of the decision-makers at the cost of the insight the sceptics carry.

"We need to mandate adoption" misunderstands the nature of the tools. Mandates backfire for tools that require judgment. A developer forced to use AI they do not trust will either accept suggestions uncritically — introducing precisely the quality risks that sceptics warned about — or review so carefully that productivity decreases. What organisations can mandate is infrastructure: context files, cost tracking, review guidelines. Build capability and let people use it when they are ready.

"Junior engineers are adopting faster; senior engineers are the problem" is true on the surface and misleading underneath. Junior engineers have less to unlearn, which accelerates adoption. They also have less ability to recognise when AI is subtly wrong, which makes their adoption riskier. Faster adoption is not inherently better. The senior engineer's caution, when it is rational rather than stale, is a quality assurance mechanism that the organisation cannot afford to dismiss.

### 14.4 Different Roles, Different Trajectories

Not every role follows the same path, and frameworks that assume uniform progression mislead.

Software developers have the highest potential ceiling — some will reach Stage 8 — but also the most significant identity investment and therefore the greatest potential for resistance. The priority is supporting voluntary progression while respecting the evolution of craft, not demanding that everyone become an orchestrator.

DevOps and platform engineers bring strong rational caution about production systems, and that caution deserves respect. The appropriate approach is guardrails for production environments with freedom to experiment elsewhere. Security-conscious resistance in this role is almost always rational, not stale.

Testers and QA engineers face a bifurcated trajectory: automation engineers often progress faster, while exploratory testers may find that AI handles the rote work they once did, freeing them for the creative testing that machines still cannot replicate.

Business and systems analysts, who often face less identity resistance because their primary skill is interpretation rather than implementation, tend to adopt for drafting and synthesis tasks with relatively little friction.

Product managers are frequently the earliest adopters — their work involves synthesis, documentation, and communication, all areas where AI provides clear productivity gains with minimal identity threat.

---

# Part VI: Conclusion

## 15. Conclusion

The resistance of experienced software developers to AI-assisted tools is not a failure of imagination or a character flaw. It is a predictable pattern that has recurred across every major technological transformation in the historical record.

The Viennese doctors who rejected Semmelweis were wrong, but they were not stupid. They were protecting their identities from an unbearable implication. The Luddites who burned looms were wrong about stopping progress, but they were right about their own futures. The surgeons who resisted anesthesia were wrong about the technology's trajectory, but they were right about its early dangers. In each case, the resistance carried legitimate insight mixed with psychological protection, and dismissing either component led to worse outcomes for everyone.

This essay has argued that resistance is not one thing and that treating it as such is the most common failure of leadership in technological transitions. Rational caution, grounded in current and accurate assessment of tool limitations, deserves guardrails and respect. Stale caution, based on outdated experience with earlier-generation tools, deserves not argument but invitation to re-evaluate. Identity-based resistance, rooted in what adoption implies for professional self-concept, deserves acknowledgment, time, and the development of new professional narratives that preserve meaning.

The mental model gap — the observation that AI tools are improving faster than assessments are updating — creates a specific category of resistance that looks rational but is based on obsolete evidence. Many developers carry assessments formed on 2023-era tools into a 2026 reality. The appropriate intervention is not arguing about past experience but inviting fresh evaluation.

Beyond resistance, the practice itself is transforming. As Yegge's framework illustrates, the progression from basic autocomplete to orchestrated multi-agent workflows is not merely "more AI" but a qualitative change in what software development means: from composition to direction, from serial to parallel, from understanding to verification, from making to evaluating. And as Khare's maturity model demonstrates, this transformation requires not just individual skill but collective infrastructure — context files, integrated APIs, governance, observability — developed in concert with individual capability.

The transformation has genuine human costs. AI fatigue, the loss of creative satisfaction, the skills mismatch between traditional and transformed practice — these are real and deserve honest acknowledgment. Not everyone will thrive in the transformed role. Some excellent developers, whose skills are superbly adapted to a mode of work that is changing around them, will find themselves in the position of the master weavers of 1811: not lacking in talent, but possessing talent for a world that is passing.

The future of software development is not AI replacing developers. It is AI-augmented teams — teams where individual skills and collective capability combine to deliver outcomes that neither could achieve alone. The practitioners who resist most strongly today may become sophisticated users tomorrow, if their concerns are respected, their expertise valued, and they receive support in evolving. Some will not make the transition. That is honest reality, and it deserves honest acknowledgment.

History suggests the technology will prevail regardless of resistance. The question is not whether but how — and whether we navigate the transition with more wisdom than the Viennese medical establishment showed Semmelweis, or the British government showed the Luddites.


