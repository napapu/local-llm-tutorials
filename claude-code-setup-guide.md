# Claude Code setup and test

1. Install [Claude Code](https://code.claude.com/docs/en/quickstart#step-1-install-claude-code).

2. Find any repository as a playground. 
> For example `https://github.com/karpathy/autoresearch`

3. `cd` into it

4. Run`export ANTHROPIC_BASE_URL=http://localhost:8001`

5. Start your llama.cpp from Step 10. in [Mac setup guide](mac-setup-guide.md).

6. Run `claude` in your playground repository.

7. Select `1. Yes, I trust this folder`

8. Ask claude `What is this repository about?`
> Now you have an agentic tool connected to your local LLM! Ask it whatever you like