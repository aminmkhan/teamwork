team work
=========

_There is no great software, only great teams!_


<!-- MarkdownTOC levels="2,3,4,5" autoanchor=false autolink=true -->

- [Our Goal](#our-goal)
- [Modeling the Problem](#modeling-the-problem)
- [Using Machine Learning](#using-machine-learning)
- [Extending the Model](#extending-the-model)
- [Visual Cues and Dashboards](#visual-cues-and-dashboards)
- [Use Cases](#use-cases)
    - [Team Alice](#team-alice)
    - [Team Bob](#team-bob)
    - [Team Mallory](#team-mallory)
- [Footnotes](#footnotes)

<!-- /MarkdownTOC -->

______________________

## Our Goal

Let's try to answer what the next best action is if we want to finish as many stories as early as possible in the iteration, and figure out how we can help team members reduce uncertainty in the iteration and optimize their workflow and planning.

## Modeling the Problem
Given that we have fixed:

- _N_ number of stories
- _M_ number of team members
- _(m, n, t, r)_ story assignments. Team member _m_ working on story _n_ for time _t_ for reward _r_ (where _r_ can represent priority, importance, etc)

Then all we ask of ML/RL is to output in what *order* to start stories `(m_1, n_1), (m_2, n_2), (m_3, n_3) …, (m_M, n_N)`, *_nothing more, nothing less_*. [1]

Now if _t_ is known, then this falls into the familiar NP-Hard job scheduling problems, and any of the efficient techniques for Knapsack family of problems can be applied [2].

But since _t_ is not known, we need to estimate _p(t|m,n)_ which need not be precise [3].

For _p(t|m)_ we can have historical time data from the project management software used by team members.

For _p(t|n)_ we have no historical time data at all for new story.

## Using Machine Learning

That leaves us to predict time for story _n_ based on existing stories, _p(t|n,N)_ (while also factoring in the estimates provided by humans).

If we tracked more attributes explicitly in the project management software for stories (code complexity, LOC, context, etc), then we can use _similarity matrix_ to base estimates on similar stories, `p(t|n,Ñ)`.

With no attributes, we need to work with _latent features_ between the stories.

The evaluation criteria for effectiveness of ML/RL is _how much planning is improved compared to scheduling based on the manual estimates._

## Extending the Model

The above model needs to be extended to handle "dependencies", and "strategy" like completing development stories before/after refinements and time-boxed stories could be preferable.

So consider in the model that stories can be dependent on other stories, or other constraints.

- The interval for story _n_ can be provided as constraints.
- By default, a story can start anytime: `(-∞, n, +∞)`
- Or a story can be limited to given interval `(t1, n, t2)`

So consider in the model that stories have preference in order for whatever reasons, such as finishing development stories before refinements or spikes can reduce uncertainty in the iteration.

- The order such as following for story `n(i)` can be provided as constraints.
- `n(i-k) < ... < n(i-2) < n(i-1) < n(i) < n(i+1) < n(i+2) < ... < n(i+k)`


The model can be further enriched with other constraints, but the point is that within the scale we are concerned with:

- Teams of 5-6 members
- Iterations of up to 25-30 stories
- Iterations/stories with dependencies/restrictions only within team
- Budget for compute time on cloud of let's say $100/team/month
- Execution time of up to 12-15 hours (running overnight to have reporting ready by start of next day)

We could even attempt to brute force the solution with exhaustive search! :smiley:

## Visual Cues and Dashboards

Identifying which visual cues and elements are the most useful for a team member may neither be obvious nor trivial.

One idea that comes to mind could be a Gantt chart like calendar view of whole iteration showing how it will play out, and where team member can drag stories around to see impact of updated plan.
(A team lead could have a view for all members, but this could be too much clutter to prove useful.)

As a side note, it could be useful for newbies to have documented for any widgets on the dashboard of project management software three examples with pictures (on good, great, ugly) along with a brief description of why iteration was so, and then potential action items.

## Use Cases


So to help brainstorm, imagine our proposed model is supported by best data scientists and implemented using the best of machine learning (ML) and reinforcement learning (RL) technology, making our system both omniscient and omnipresent, what dashboards and visual cues would help the team members most in their work then? :wink:

For instance, how can the dashboards and reporting in the solution can come in handy for each of these example scenarios.

### Team Alice

Always perfect, never over-commits or under-delivers. All stories get short duration, easy to plan. Have squeezed all uncertainty out of their iterations.

### Team Bob

Great work, but with the usual uncertainties, miss to meet 10% of their commitments for the iterations.

### Team Mallory

Good team, but hitting issues because of complex project. Lot of uncertainty in scope, in iteration planning. Stories with longer duration to be meaningful. Meet 50% of their commitment.

## Footnotes

- [1] This is generic enough to model other interventions like dropping and stopping a story, or adding to or removing members from a story
- [2] Though considering that story assignments are limited to teams for us,  `|(m, n, t, r)| << |M ✕ N|`, and hence search space is `<< O(|M ✕ N|!)`
- [3] Since we are concerned with relative order of stories, and not their absolute time.
