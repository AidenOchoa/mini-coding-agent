# Q-learning demo evidence

## Skill activation

These entries come from `skills.log` for session `20261119-124227-0d22aa` on Tuna.

```jsonl
{"time": "2026-11-19T19:42:33.797666+00:00", "event": "prompt", "session": "20261119-124227-0d22aa", "depth": 0, "source": "prompt", "prompt": "/check-vacuum Run python scripts/check_project.py from the workspace root. Then read context/environment.md, scripts/test_q_learning.py, and learning_todos.py. Complete all five TODOs in learning_todos.py. For this run, work directly on the implementation and skip delegation. Edit only learning_todos.py. After saving, summarize your changes and stop"}
{"time": "2026-11-19T19:42:33.798961+00:00", "event": "similarity_check", "session": "20261119-124227-0d22aa", "depth": 0, "source": "prompt", "method": "exact_name", "scores": {"check-vacuum": 1.0}, "threshold": 0.25, "best": "check-vacuum", "selected": "check-vacuum"}
{"time": "2026-11-19T19:42:33.799828+00:00", "event": "skill_executed", "session": "20261119-124227-0d22aa", "depth": 0, "skill": "check-vacuum", "description": "Check the Lab 5 notebook and use focused specialists to review the environment, requirements, and code features.", "trigger": "explicit", "score": 1.0, "path": ".claude/skills/check-vacuum/SKILL.md"}
```

The score of `1.0` came from selecting the skill by name. The checker reported
`Lab 5 checks: PASS` and listed all five unfinished TODOs. The agent then reached
its tool step limit.

## Applied patch

In a separate, focused run, Qwen 3.5 27B proposed this `patch_file` edit to
`learning_todos.py` for `derive_policy`. After manual approval, session
`20261119-130719-02d573` recorded the tool result `patched learning_todos.py`.

```diff
-    raise NotImplementedError("TODO: implement derive_policy")
+    policy = {}
+    for state, actions in q_values.items():
+        best_action = max(actions, key=actions.get)
+        policy[state] = best_action
+    return policy
```

## Final verification

After all five functions were implemented through focused follow-up prompts,
`python q_learning_demo.py verify` passed all 31 tests on Tuna. The evaluation
reached the recharge station in 500 of 500 episodes with exploration disabled.
The full metrics are in [results.json](results.json).
