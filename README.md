# Livewire Sortable

A plugin/wrapper around [Shopify's sortable package](https://github.com/Shopify/draggable/tree/master/src/Sortable). It makes implementing sortable interfaces super simple using Livewire.

## Installation

```
npm install livewire-sortable

or

yarn add livewire-sortable
```

## Usage

For simple layouts that only require simple sorting like a todo list, add the `wire:sortable`, `wire:sortable.item`, and `wire:sortable.handle` attributes to your markup as follows.

```html
<ul wire:sortable="updateTaskOrder">
    @foreach ($tasks as $task)
        <li wire:key="task-{{ $task->id }}" wire:sortable.item="{{ $task->id }}">
            <h4 wire:sortable.handle>{{ $task->title }}</h4>
            <button wire:click="removeTask({{ $task->id }})">Remove</button>
        </li>
    @endforeach
</ul>
```

For creating a nested layout with draggable groups with draggale items inside each group, similar to Trello, add the `wire:sortable`, `wire:sortable-group`, `wire:sortable.item`, `wire:sortable.handle`, `wire:sortable-group.item-group`, and `wire:sortable-group.item` attributes to your markup as follows.

```html
<div wire:sortable="updateGroupOrder" wire:sortable-group="updateTaskOrder" style="display: flex">
    @foreach ($groups as $group)
        <div wire:key="group-{{ $group->id }}" wire:sortable.item="{{ $group->id }}">
            <div style="display: flex">
                <h4 wire:sortable.handle>{{ $group->label }}</h4>

                <button wire:click="removeGroup({{ $group->id }})">Remove</button>
            </div>

            <ul wire:sortable-group.item-group="{{ $group->id }}">
                @foreach ($group->tasks()->orderBy('order')->get() as $task)
                    <li wire:key="task-{{ $task->id }}" wire:sortable-group.item="{{ $task->id }}">
                        {{ $task->title }}
                        <button wire:click="removeTask({{ $task->id }})">Remove</button>
                    </li>
                @endforeach
            </ul>

            <form wire:submit.prevent="addTask({{ $group->id }}, $event.target.title.value)">
                <input type="text" name="title">

                <button>Add Task</button>
            </form>
        </div>
    @endforeach

    <form wire:submit.prevent="addGroup">
        <input type="text" wire:model="newGroupLabel">

        <button>Add Task Group</button>
    </form>
</div>
```
