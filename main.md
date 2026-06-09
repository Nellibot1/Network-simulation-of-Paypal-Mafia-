# Introduction

Does the technology we use shape how we think? If yes, who shapes the
technologies?

These questions have become increasingly important as digital platforms
and large language models move from being simple tools of communication
to becoming central infrastructures of information. Systems such as
ChatGPT, X and other platform-based technologies increasingly mediate
how people search for knowledge, encounter political views, as well as
form and influence opinions ([@gandini2025algorithmic]). Instead of
solemnly giving information, these systems can generate and also
structure them. But there are also other technologies used in the public
sectors such as Palantir Gotham a platform, designed for intelligence
defense and law enforcement use ([@montevirgen2026palantir]).
Technologies like these can even have regulatory implications because
they potentially affect vulnerable citizens ([@ulbricht2024palantir]).

This gives companies and individuals behind them a quiet but significant
form of social and political influence. As [@hancock2025techoligarchy]
argues, the risk posed by contemporary tech oligarchy is not only that
individual billionaires hold extreme wealth, but that they also control
key layers of the information infrastructures on which democratic
societies depend.

This concentration of technological and informational power has become a
concern in contemporary politics and sociology. Tech elites increasingly
exercise authority across boundaries that traditionally separate
markets, infrastructure, media, and also the state.
[@atal2026oligarchic] describe this development as "oligarchic
sovereignty": a structure in which privately controlled technological
systems become part of the political architecture. In this context, the
owners and founders of major technology companies do not just
participate in markets. Rather do they shape the conditions under which
information and political narratives spread, as well as how citizens
encounter public life. This makes it necessary to ask not only what
technologies do, but also who is connected to the technologies that
increasingly shape the public.

A particularly relevant case is the so called "PayPal Mafia," the
network of founders and early employees of PayPal after the company's
sale in 2002 ([@hoffman2013startup]). Individuals of it are part of an
ecosystem of companies, investment funds, platforms,as well as political
projects. Individuals such as Elon Musk, Peter Thiel, David Sacks, Reid
Hoffman and others have become associated with companies and
institutions that now occupy central positions in technology, media,
artificial intelligence, defense, venture capital, and government
([@brehse2024paypalmafia]).

## Background

Past work of Pitti (2004) describes and argues that the group's shared
origin created a durable network advantage, but does not test whether
PayPal-affiliated actors actually have structurally advantaged positions
or whether they are especially effective at spreading influence through
the technology political network. Research on Silicon Valley shows its
robust complex system of innovation which is supported by social
networks of interdependent economic agents in which the Venture Capital
firms function specifically ([@ferrary2009venturecapital]).

Other research on the business elite shows that the ties people form
early on their careers leave a lasting impact, so much that well placed
actors keep outperforming others who are just as talented but less
connected ([@uzzi2016formation]). One reason this happens is homophily,
the tendency for similar individuals to connect to one another, which
produces and reinforces structures that channel influence across the
connections that already exist([@mcpherson2001birds]). Eigenvector based
measures, assess structural bias in how information and influence are
aggregated across a network ([@bienenstock2022eigenvector]). A very
centralized network is one where a few nodes decide a lot about what the
whole group ends up paying attention to. At the largest scale,
[@atal2026oligarchic] argue that technology elites now reach between
markets, infrastructure and the state, to the point where privately
owned systems and platforms start to shape global order itself.

Position in a network is one thing,but the way influence actually
diffuses through it is another. [@granovetter1978threshold] showed that
collective behaviour depends not only on what individuals want but on
thresholds, of how many others have to act before a given person joins
in. [@granovetter1978threshold]'s threshold model established that
collective outcomes depend not only on individual predispositions but
also on the distribution of thresholds and the order in which actors are
exposed to active others.

[@watts2002simple] extended this logic to networks, showing that global
cascades occur only under specific structural conditions, while
[@kempe2003maximizing] then asked which starting nodes spread influence
the furthest, which means that the choice of where a cascade begins can
matter just as much as the shape of the network.
[@unicomb2018threshold]added that the weight of the ties matters too,
since the strength of a connection decides whether the influence adding
up on a node is enough to push it over its threshold. In total this
implies that a small set of highly central and strongly tied actors can
seed disproportionately large cascades. Although the PayPal phenomenon
has been described it has not been modelled yet. Furthermore were
diffusion and centralization frameworks rarely been applied to a
concrete technology and political elite. This paper tries to explore
this gap.

The research question we pose therefore is: whether the actors tied to
the PayPal network sit in structurally advantages positions inside a
technology and politics influence network, and whether they set off a
wider spread of influence comparable group of randomly chosen elite
actors under a weighted threshold model. To explore this question we
simulate a a weighted threshold diffusion based on simulated network.

# Methodology

## Building the network

The model is implemented in Python using the NetworkX library. The
network is a directed graph of 25 nodes and 42 edges.

Nodes represent heterogeneous actors of four types: persons (e.g. Elon
Musk, Peter Thiel, Donald Trump), organisations, plattforms or venture
capital firms (e.g. PayPal, Twitter/X, Founders Fund, OpenAI), and
institutions (the U.S. government, the defence system, and "the
Public"). Each node is also assigned to one of nine subgroups, such as
paypal mafia, tech, governmentpolitics, AI infrastructure,
investmentfirm and more. These serve both as visual categories, as well
as filters for evaluating whether cascades reach governmental or public
actors. Directed edges encode relations such as founder/executive ties,
investment links, political support, government contracting or
information flows. Each carry a weight between 0- 1 to showcase
relationship strength.

\
Three node-level parameters drive the model.

- Power (0--1) represents an actor's capacity to influence others.

- Threshold, defined as the number or proportion of others who must make
  one decision before a given actor is activated.

- Infrastructure power, which captures the degree to which a node
  functions as a platform, surveillance, or AI system of broad societal
  reach.

The values were formulated by us rather then extracted from a dataset as
these concepts are difficult to quantify. For example are Elon Musk,
Donald Trump and the U.S. government each assigned with the maximum
power of 1.00, while ChatGPT receives the maximum infrastructure power.
This is therefore a synthetic, theory-driven network,which is if further
addressed in the Limitations.

<figure id="fig:placeholder" data-latex-placement="h">
<img src="./Network.png" style="width:100.0%" />
<figcaption>People and organizations of tech-political
network</figcaption>
</figure>

## Centrality measures

To assess structural position of where each actor sits, the network is
first turned into an undirected version. Because betweenness and
closeness require a notion of distance, every edge is given a distance
equal to one divided by its weight (distance = 1/weight). A stronger tie
then counts as a shorter path, which fits the weighted view of contagion
by [@unicomb2018threshold].

Four standard measures are computed ([@bienenstock2022eigenvector]):
degree centrality (direct connectivity), betweenness centrality
(brokerage across shortest weighted paths), eigenvector centrality
(connection to other well connected nodes, computed with edge weights)
and closeness centrality (overall proximity to the network). Reporting
all four allows the analysis to distinguish actors who are just well
connected from those who occupy genuine bridging or influential
positions.

## Weighted threshold diffusion

Influence spread is modelled with a weighted threshold diffusion process
that combines the threshold logic of [@granovetter1978threshold] and
[@watts2002simple] with the weighted-influence and seed-selection
concerns of [@unicomb2018threshold] and [@kempe2003maximizing]. At each
step, an inactive node aggregates influence from its directed
predecessors. For a neighbour, the contribution is the edge weight
multiplied by an influence capacity equal to (power + 0.5 ×
infrastructure power).The 0.5 coefficient is the model's infrastructure
coefficient. The node computes an adoption ratio equal to the active
predecessors contribution divided by the total contribution of all
predecessors. It activates when this ratio meets or exceeds its
threshold.

- *Active Influence = edge weight x (power + 0.5 x infrastructure power*

- *Total possible influence: total influence = edge weight x (power +
  0.5 x infrastructure power)*

- *Node adopts when:*
  $$\frac{\textit{active influence}}{\textit{total influence}}
      \geq
      \textit{threshold}$$

The process iterates until no further node activates or 20 step limit is
reached. This is a linear threshold style with weighted, with attribute
influence. Infrastructure rich nodes such as ChatGPT or state
institutions therefore propagate influence more strongly than ordinary
actors. Afterwards four analyses are done. The first starts the spread
from the three core seeds the PayPal actors Musk, Thiel and Sacks, who
are treated as already active. The second compares seven scenarios with
different starting groups,such as the PayPal core, the Musk and Thiel
ecosystems, the AI infrastructures, platforms and public group,venture
capital and defense group,as well as a government group. For this we
recorded how many nodes end up active and whether the government and the
public nodes are reached. The third is a random baseline that runs the
same spread 1,000 times from random groups of three people, which gives
us something to compare the PayPal result against. A final sensetivity
tests showcases how sensitive the model is, by rescaling every threshold
by 0.75, 1.0 and 1.25 and by setting the infrastructure coefficient to
0.0, 0.5 and 1.0. A fixed random seed of 67 keeps the runs reproducible.

# Results

## Centrality and structural advantage

<figure id="fig:placeholder" data-latex-placement="h">
<img src="./output.png" style="width:100.0%" />
<figcaption>The ten nodes with the highest betweenness, coloured by
group. PayPal actors, shown in light blue and government actors hold
leading bridge positions</figcaption>
</figure>

The centrality analysis (Figure 2) shows that the most structurally
advantaged positions are shared between PayPal affiliated actors and
government institutions rather than being a monopol of either side.

<figure id="fig:placeholder" data-latex-placement="h">
<img src="./Screenshot 2026-06-08 at 19.12.49.png"
style="width:100.0%" />
<figcaption>Centrality scores for the ten nodes with the highest
betweenness</figcaption>
</figure>

Figure 3 shows that Elon Musk has the highest betweenness centrality
(.232) and the highest degree (.292), which makes him the network's main
bridge. Right behind him come the US government at .217 and Peter Thiel
(.194), with the PayPal company fourth (.192). On the eigenvector
measure, which can be read as influence over how information is pooled
(), Donald Trump comes first (.370), just ahead of Musk (.366), Thiel
(.330) and Sacks at (.328) while the US government and PayPal tie are
lower (both at .306). Closeness centrality tells a similar story, with
Trump at .344, Musk at .340 and Thiel at .339. Across every measure,
three PayPal actors and the PayPal company keep showing up near the top,
but the main government nodes do as well. The showcases the ties of the
actors are and how that reaches across technology and the state at the
same time.

## Starting from the PayPal core

Starting the spread from Musk, Thiel and Sacks sets off a fast and wide
cascade. From the three seeds the process switches on 11 nodes at step 1
and 15 of the 25 nodes by step 2, after which it stops. The first wave
pulls in the immediate business ties, PayPal, Founders Fund, Palantir,
SpaceX, Tesla and Twitter/X, along with two political actors J.D. Vance
and Donald Trump. The second wave reaches Anduril, the Trump Campaign,
the US government and the defense state. In other words, the influence
moves out of the technology world and into government in just two steps
(Figure 4 and Figure 5). Several nodes switch on with their ratio well
past the threshold. Palantir, Tesla, Twitter/X and Vance each reach a
ratio of 1.0, which reflects how dense and strongly weighted the ties
among the PayPal linked nodes are.

<figure id="fig:placeholder" data-latex-placement="h">
<img src="./4.png" style="width:100.0%" />
<figcaption>How many nodes are active at each step when the spread
starts from the PayPal core. The cascade levels off after two steps at
15 of 25 nodes.</figcaption>
</figure>

<figure id="fig:placeholder" data-latex-placement="h">
<img src="./2.png" style="width:100.0%" />
<figcaption>The 25 node technology and politics network. Red nodes are
the ones that switch on when the spread is started from the three core
PayPal actors, grey nodes stay off.</figcaption>
</figure>

## Comparing different starting groups

Comparing seed sets (Figure 6 and 7) showcase differences in spreading.
Although the PayPal core is one of the most effective seeds, activating
15 nodes and reaching five governmental nodes including the U.S.
government, the venture capital/defense tech seeds (Andreessen,
Horowitz, Luckey) diffuse furthest of all, activating 19 nodes. The
Thiel ecosystem reaches 10 nodes, while platform , Musk, AI and
government seeds spread less widely. Furthermore does the AI
infrastructure and government only seeds produce no cascade (final
active count equal to the three seeds, zero additional steps).

Despite the high values of OpenAI, ChatGPT, and the state, these nodes
occupy positions in the simulated network from which influence does not
propagate under the models directed structure as they do not reflect
ties but rather influence on society itself which is not part of the
simulated network. Even though OpenAI, ChatGPT and the state carry very
high power and infrastructure values within the network the directions
of the edges leave them in places from which influence simply doesn't
flow. It is the position of the starting group, rather than the power
that drives the spread, which also is reflected by the seed selection
problem described by [@kempe2003maximizing].

<figure id="fig:placeholder" data-latex-placement="h">
<img src="./6.png" style="width:100.0%" />
<figcaption>Final number of active nodes for each starting group. The
venture capital and defense group spreads the furthest, with the PayPal
core in second place.</figcaption>
</figure>

<figure id="fig:placeholder" data-latex-placement="h">
<img src="./Screenshot 2026-06-08 at 19.38.57.png"
style="width:100.0%" />
<figcaption>Spread outcomes for each starting group.</figcaption>
</figure>

## Random baseline comparison

Compared with the 1,000 random groups of three people, the PayPal
cascade of 15 nodes sits above the random average of about 11.6 nodes.
It is therefore in the upper middle of the spread rather than far out at
the edge (Figure 8). The PayPal group beats 63.3 percent of the random
groups outright and matches or beats 66.7 percent of them. Across the
random runs the US government is reached about 51 percent of the time,
while the Public is reached only about 11 percent of the time, which
showcases that capturing government is fairly easy in this network while
reaching the wider public is much harder. So the PayPal advantage is
real but modest in this simulation. It does better than most random
elite three seeds, yet a lot of random groups match or beat it and at
least one carefully chosen group does better.

<figure id="fig:placeholder" data-latex-placement="h">
<img src="./baseline.png" style="width:100.0%" />
<figcaption>How big the cascade gets across 1,000 random groups of three
people, with the PayPal result marked by the red line. The PayPal group
beats 63.3 percent of the random groups.</figcaption>
</figure>

<figure id="fig:placeholder" data-latex-placement="h">
<img src="./sensetivity1.png" style="width:100.0%" />
<figcaption>Change of thresholds.</figcaption>
</figure>

## Sensitivity

The model reacts to the threshold (Figure 9) but barely notices the
infrastructure coefficient. Lowering every threshold by 25 percent makes
the cascades slightly larger,the PayPal core rises from 15 to 16 and the
Musk ecosystem from 7 to 9. Raising every threshold by 25 percent
shrinks them, so the PayPal core falls to 12 and the Thiel ecosystem
from 10 to 6. This is the threshold dependence we would expect described
by [@granovetter1978threshold] and [@watts2002simple]. The
infrastructure coefficient behaves slightly different. Setting it to
0.0, 0.5 or 1.0 leaves every scenarios final cascade size unchanged.
Because the infrastructure term shows up in both the top and the bottom
of the adoption ratio, rescaling it does not change which nodes cross
their thresholds in this particular network. That is an important null
result,that we further elaborate on.

# Discussion

The model of the simulation aligns with the central claims done by
previous literature, that the Paypal figures do sit in strong positions
([@uzzi2016formation],Pitti (2004)) . Elon Musk is positioned as the
main bridge, and Thiel, Sacks and the PayPal company cluster are at the
top of the centrality rankings done in the simulation. The fast two step
cascade that runs from the PayPal core into government and defense nodes
is a small and simplified version of the tactical move from Silicon
Valley to Washington that Pitti (2004) describes. Showcasing how
technology elites become more interwoven into the government and the
general united state as described by [-@atal2026oligarchic]. The
political views these actors hold might explain the subtle \"vibe
shift\" experience in the US, as political actors such as JD Vance and
Donald Trump have had campaigns partially funded by Tech Actors (Peter
Thiel).

At the same time the strongest positions in the model are not held by
the technology cohort alone. Trump leads on eigenvector and closeness
centrality, as well as the US government which keeps turning up among
the most central nodes. The way eigenvector centrality piles up in a
handful of mixed technology and government nodes is a sign of structural
bias in how influence is pooled in the network simulation
([@bienenstock2022eigenvector]). The bias often favours an elite that is
part technology and part politics, not just technology. The scenario and
baseline results show that that PayPal is not to be seen as that unique.
The venture capital and defence group spreads further.Furthermore does
the PayPal group beat only about 63 percent of random elite trios. The
edge is there but modest, it also stems more from good positioning than
from anything distinct about \"PayPal Mafia\" as such. That fits
[@kempe2003maximizing], that explain that the spread depends on where a
cascade is seeded, as well as [@watts2002simple], that states that
cascades depend on structural conditions rather than on the fame of any
single actor. The homophily built into the network also explain the
speed of the cascade. The PayPal linked nodes are joined by dense,
strongly weighted ties, with nodes that are similar and connected
leading to each others crossing of thresholds fastly.On the other hand
the same dense structure traps influence inside the cluster. Groups that
start outside the dense core, like the AI infrastructures or the
government only group, fail to spread at all in the simulation. So
homophily shows up both ways. All together, the model supports the
qualitative literature's emphasis on elite networks and cross domain
influence (politics and technology). Although the model also shows that
the influence of a network is not based on inherent power per se but
rather simply on its structure and placement.

# Limitations

Our project shows how influence could spread in a network of tech CEOs,
companies and political actors, but the model has several important
limitations. Many of them come from the fact that we built the network
by hand and chose the parameters ourselves, without using a large
empirical dataset. We are going to discuss the most important points
below.

## The network is built by hand and is incomplete

We selected the nodes (people, companies and institutions) based on our
own research of the \"PayPal Mafia\" and the current US tech-political
landscape. This means that the network only reflects a small part of
reality. Other important actors, such as Microsoft, Google, Meta, Jeff
Bezos with its Amazon or non-US tech elites, are not included. A
different selection of nodes would probably change the results. The
network should therefore be seen as one possible representation, not as
the full picture.

## The network is static

The graph represents a single snapshot in time. In reality,
relationships between people and companies change. Alliances can form,
weaken or even break. For instance does the public conflict between Elon
Musk and Donald Trump in 2025 show that political alignments are not
stable. Our model cannot capture these changes over time. We are limited
here with the current situation.

## The edge weights are not measured empirically

We assigned the weights of the edges (for example 0.90 for \"founder\",
0.45 for \"$vc  ecosystem$\") based on our own intuition about how
strong a relationship is. We did not use empirical sources such as the
number of co-investments, board memberships or news co-mentions.
Different weights would lead to different cascades, so the exact
numerical results should not be over interpreted.

## Influence is not directly measurable.

Concepts such as \"power\", \"threshold\" or \"infrastructural
importance\" cannot be observed directly in the real world. There is no
dataset that says \"Elon Musk has a power score of 1.00\". We therefore
had to translate these abstract sociological concepts into numerical
values between 0 and 1 ourselves, based on plausible reasoning. This is
a general limitation of computational modelling of social systems: many
of the most interesting variables are latent and have to be
approximated.

## Limited availability of empirical data on elite networks.

There is no public, complete dataset that lists all the relevant
relationships between tech CEOs, their companies, investors, and
political actors. Information about who invests in whom, who advises
whom, or who is politically aligned with whom is spread across news
articles, interviews, and partial databases such as LittleSis or
Crunchbase. Because of this, we had to build the network by hand, based
on publicly known facts. This is a structural limitation of working on
elite influence: the people we study often have an interest in keeping
their relationships informal or non-public.

## Social influence is more complex than any model can capture.

Real influence between people and institutions includes emotions, trust,
media framing, personal history, financial dependencies, ideology, and
many other factors. A simulation always has to reduce this complexity to
a small number of mechanisms (in our case $->$ weighted neighbors and a
threshold). This simplification is necessary to make the model
understandable and computable, but it means that the simulation can only
represent one specific aspect of influence and not the full social
reality.

## Summary of Limitations

The main limitations of our project was not coding errors or wrong
assumptions, but the overall structural limitations of gathering data of
elite influence with a network simulation. Because data of elite
relationships is mostly incomplete and hardy accessible for the public
(key variables like power, influence, threshold are not directly
observable). The simulation results cannot be validated against a clear
real world data truth base. Our simulation should therefore be
understood as a theoretical and exploratory tool that makes assumptions.

# Conclusion
The research question of this paper was whether actors connected to the PayPal network sit in structurally advantaged positions inside a technology and politics influence network and whether they spread influence further than a comparable group of randomly chosen elite actors.

For the first question the answer based on our simulation is yes but with some restrictions. The centrality analysis shows that Elon Musk has the highest betweenness and degree centrality of the whole network and Peter Thiel, David Sacks and the PayPal company also rank among the top nodes on every measure. So the PayPal actors do hold strong bridging and connecting positions. However the same top positions are also occupied by Donald Trump and the US government. Trump even leads on eigenvector and closeness centrality. This means the structural advantage in our model is not held by the tech actors alone but is shared with central political actors.

For the second question the result is more careful. When we start the cascade from the three PayPal seeds (Musk, Thiel and Sacks) the influence reaches 15 of 25 nodes in only two steps including government and defense nodes. Compared to 1000 random groups of three persons the PayPal group beats about 63 percent of them and matches or beats about 67 percent. So the PayPal seeds do spread influence further than the average random elite group but the advantage is moderate rather than singular. The scenario comparison even shows that a different group (the venture capital and defense tech actors) reaches 19 nodes which is more than the PayPal core itself. This tells us that the spread depends more on the structural position of the starting group than on the PayPal origin itself.

The sensitivity analysis adds another point to the result. The cascade size reacts as expected when we change the thresholds but the infrastructure coefficient (0.0 0.5 or 1.0) does not change any final cascade size. Since this term appears in both the numerator and the denominator of the adoption ratio it cancels out for this network. We see this as a useful finding because it pushed us to look at our own model more critically and to see that not every parameter we added actually does work.

Overall the simulation supports the qualitative claim from the literature that the PayPal network sits in a strong position between technology and politics but it does not show that they are uniquely powerful. The cascade behaviour is driven by structural position and dense ties between similar actors not by a "PayPal Mafia" identity on its own. The model also shows the value and the limits of using a simulation for this kind of question. A formal model forces us to write down our assumptions and to test them but it can also only be as good as the data and the parameters that we put in.

So coming back to the questions we started with. Yes the technology we use can shape how we think and yes the people behind these technologies do hold structurally strong positions in our simulated network. But the answer to "who shapes the technologies" is not a single closed group like the PayPal Mafia. It is a wider mix of technology actors and political institutions whose influence depends on where they sit in the network and how strongly they are connected to the rest.

[^1]: [GitHub
    repository](https://github.com/Nellibot1/Network-simulation-of-Paypal-Mafia-)
