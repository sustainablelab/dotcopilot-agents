# About

Safe agents for use with GitHub copilot.

# GitHub copilot default agents

GitHub copilot has three agent selections: `Agent mode` (`Ctrl+Shift+Alt+I`),
`Ask mode` (`Ask.agent.md`), and `Plan mode` (`Plan.agent.md`). These agents
are configured by a `.agent.md` file (though I don't know where that file is
for the `Agent mode`).

The `.agent.md` file has two main parts: a list of VSCode tools the agent has
access to, and a list of rules that tell the agent how it should behave. While
a rule cannot override a tool omission (e.g., without the `edit` tool, the
agent cannot create/edit files, no matter what the rules say), it is important
(?) that the rules agree with the tool list.

The `Configure Tools...` button lets you disable tools (such as the ability to
read terminal output or execute files), but those changes are temporary to that
specific session. The way to make permanent changes is to put your `.agent.md`
files in `~/.copilot/agents`.

# Why did I make custom agents

In the default `Agent mode`, all of the boxes in `Configure Toosl...` are
checked, giving the LLM the ability to write and execute files. This level of
access is insane.

The default `Ask mode` has the ability to read terminal output. This means the
agent may inadvertently read information outside the VSCode Workspace (e.g., if
the terminal prints the contents of a file).

# Safe agents

I made "Safe" versions of the GitHub copilot `Ask mode` and `Agent mode`. This
is probably too much friction for most people, but it is an improvement over my
existing setup.

I have been using ChatGPT for months purely through a web browser interface. To
work on code, I upload `.tar.gz` (zip) files to ChatGPT. It makes its own
version of the code and hands back a `.tar.gz` file and a list of which files
it changed/added. I then run a diff on each file to review its changes.

I created these "safe" agents so that I could cut out those intermediate steps
of compressing/extracting the repo. But I still do not want the agent to make
any direct edits. The `safe-agent.agent.md` file (my `Safe Agent`) does not
have the VSCode tool `edit/editFiles`. Instead, the rules section tells it to:
"Create a complete proposed replacement under
`.agent-proposals/<original-relative-path>` using the file-creation tool."

## Safe Ask

My `Safe Ask` is a more narrowly scoped, workspace-focused version of `Ask`. It
has explicit boundaries and fewer VSCode tools.

## Safe Agent

My `Safe Agent` has the same tools as my `Safe Ask`, plus `edit/createFile`,
`edit/createDirectory`, which allow it to create files and directories. But it
is still not allowed to edit files.
