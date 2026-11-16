# Judge with a decision model first, chosen by testing on our own claims

Whether a passage carries a claim (an overstated claim, failure (c)) is the one part no script can settle. Judge sends each citation's claim and passages to a decision model as a yes/no question and reads its support score: the probability of "yes", or the model's native score, on a 0–1 scale. At or above the model's threshold the citation is Supported. Below it, an Opus subagent reads the whole cited section, gives a verdict (Supported, Overstated or Not supported) and proposes a fix. The user reads only what is flagged. Judge is its own command; verify makes no AI calls.

## Considered options

- **Haiku first, then Opus, both as subagents.** Haiku can't report a probability, only a verdict, so the threshold the user asked for has nothing to act on. Kept only as a test candidate.
- **A 1–100 rating written by an LLM.** Rejected: a number written as text is badly calibrated, bunches on round values and changes between runs. A decision model's probability comes from the model itself.
- **Escalate only an uncertain band, e.g. 0.15–0.85.** Rejected: a confident "no" is a finding that needs a proposed fix anyway, so only a confident "yes" may skip Opus. One threshold per model.
- **Pick by leaderboard.** Rejected. On LLM-AggreFact (llm-aggrefact.github.io) the top models sit within 3 points of each other, measured on news and dialogue rather than paraphrased book passages, and none goes much above 80%.

  | Candidate | AggreFact | Runs |
  |---|---|---|
  | nimble-v4-max | 79.5 | Bespoke Labs API |
  | Clef 27B | 79.0 | Locally, if it runs on an M4 Max with 36 GB |
  | Jev 1.14 (docs.typesafe.ai) | 78.7 | TypeSafe API, `noul` yes/no or `Score` questions, $0.042 per million input tokens, output free |
  | Bespoke-MiniCheck-7B | 77.4 | Ollama; non-commercial licence |
  | MiniCheck-Flan-T5-L | 75.0 | Locally, CPU or Apple GPU; MIT |
  | HHEM-2.1-Open | 71.8 | CPU; native score 0–1 |

- **Choose by testing on our own claims (chosen).** The Places report's claims, each with its real passages, plus an overstated version of each written by Opus ("often" turned into "always"), with Opus verdicts as the reference. Candidates: Jev, nimble-v4-max, Clef 27B if it runs locally, MiniCheck-Flan-T5-L, Haiku. The winner lets the fewest overstated claims through with a score above its threshold, at a cost the user accepts. The same run sets the threshold.

## Consequences

- The model and the threshold are arguments, e.g. `/citation-check triage=jev-1.14.0 judge=opus`.
- API keys come from the environment (`TYPESAFE_API_KEY` and the like).
- Expect about one wrong answer in five from any first-pass model; the threshold and the Opus escalation exist for that.
