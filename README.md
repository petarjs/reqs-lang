# ReqsLang

ReqsLang is a simple and flexible language for defining project requirements and tasks.

The idea behind it is to have a `.reqs` text file where you define requirements of your project. That way it's right where your code is, and it's checked in to your VCS.

I created a plugin for syntax highlighting for Sublime 3. You can find it [here](https://github.com/petarslovic/sublime-reqs-language).

There are some aspects of this that are not so ideal, like collaboration - you'll get the changes your colleagues make when you pull code from Git, instead of right away, as with other tools. Also, ReqsLang is probably best fitted to small projects.

There is no parser or GUI for this, although it'd be cool to make one and integrate it.

I'd love to hear what people think about this, and to get a discussion going on how we can improve this. Hopefully at least one other person will find this useful. That's enough to warm my heart. <3

## Example

```
---
Some cool project
This is a description of the project.

- GitHub
  https://github.com/petarslovic/reqs-lang

- Milestones
  [beta], [release]

- Members
  @aleksandar, @petar
---

[beta]
  - [x] Make font bigger
    @petar #front-end --deadline 23.02.2016.

    Make it like 100px. #bigtext

  - [ ] #12 Fix colors on header
    @aleksandar #front-end #urgent --deadline tomorrow --priority max

    We need to fix stuff here so that it's all nice.
    Maybe even so good that we don't really right now.

    --comments
      @aleksandar - I was thinking maybe we should use the color green. (23.02.2016. 12:32pm)
      @petar - I don't know, I like red better. (23.02.2016. 12:33pm)

    --references
      #11 - this is the one where we first changed color

    --dependencies
      #10 - this has to be done in order for #12 to work

[release]
  - [ ] Make sure the build works
    @petar --lets-review

    We need to put everything on server.

  - [ ] Test
    @aleksandar #important

    We need to test stuff.
```

## File name

It can be called `.reqs` or `.requirements`. Alternatively, if you have a lot of tasks, you can split them into various files, like `user-list.reqs`, `movie-management.reqs`, etc.

## Indentation

I don't think there should be hard rules about indentation. Use whatever feels right. The example above is what feels right to me.

## Elements

### Project description

At the very top of the file, you can add info about the project, enclosed in `---`. Standard things to list are the project name, description, any links (like link to github repo), list of members, list of milestones.

### Milestones

Milestones are defined between brackets. We can see in the example that there are two milestones - `[beta]` and `[release]`. The milestones need to be defined at a start of the line.

### Task

Task is a basic unit of ReqsLang. It can be defined as a task, user story or requirement. 

#### Task Title

Task title starts with a dash - `-`. At the beginning of the line we have a "checkbox" that indicates if the task is complete - `[ ]` for incomplete tasks and `[x]` for complete tasks. You can use any character you like instead of the `x` inside of the brackets. After the `[x]` you can add a task identifier in the form of `#ID`. ID can be a number of anything you want.

#### Mentions

Mentions are defined in the second line of the task, which is reserved for metadata. Mentions have a simple format - `@name`. They are used to indicate who is responsible for the task.

#### Tags

Tags are defined as a hash - `#`, followed by tag name. They are used for adding labels to a task, like `#front-end` or `#important`. They group tasks into logical buckets. In the rest of the task text, `#` tags can also be used. One cool use could be to link to other tasks, like `#142`.

#### Custom Variables

Custom variables can be used to add even more context to a task. They are defined with `--` at the beginning and written in snake case. So `--awaiting-review` or `--deadline` are fine. They can be followed by a value, like `--deadline 2015-04-01`. Value of the variable can be whatever you want, like this date for example, use any date format that you want. Because #flexibility. Custom variables can also be used without a value, in which case they just state a fact about the task. For example `--has-complications` or `--breaks-build`.

### Task Text

Task text is the description of the task. It can be as long or as short as you like. You can omit it entirely if you want.

### Custom Info Area

This is an area for even more information. 3 types of info are currently defined, but you can add custom ones that you may find useful.

They are defined by a title and body. Title is in the format of a custom variable - so `--comments`. The body part is variable.

#### --tasks

We can add subtasks to our task in this section. We can nest tasks as deep as we want.

```
    --tasks
      - [x] Open the editor
        @petar
      - [ ] Navigate to the right file
      - [ ] Move the cursor to the right position
      - [ ] Type some text
```

#### --comments

In the body, each line is a comment. A comment shows the user who created it (in form `@name`), the comment text and the date in parenthesis - (23.02.2016.). Again, the date can be in any format.
`@aleksandar - I was thinking maybe we should use the color green. (23.02.2016. 12:32pm)`

#### --references

Here we can list tasks related in some way to this one.

`#11 - this is a reference to the task 11`

#### --dependencies

This should be used to show a dependency on a different task. That means that tasks listed there need to be completed before starting work on that task.

`#10 - this has to be done in order for #12 to work`