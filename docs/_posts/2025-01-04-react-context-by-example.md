---
title: "React Context By Example: Managing State in RPGMyLife"
date: 2025-01-04 12:00:00 -0500
categories:
  - Programming
  - React
tags:
  - react
  - context
  - state management
  - web development
  - typescript
excerpt: "A practical exploration of React Context through building RPGMyLife - when to use it, how it works, and real implementation examples."
classes: wide
toc: true
toc_label: "Context Topics"
toc_icon: "cog"
published: false
---

## Understanding React's Foundation

To understand Context in React, we need to start with React's core philosophy. React embraces functional programming principles to create predictable, reusable, and performant user interfaces. At its heart, React applications are trees of pure functions (components) that transform input (props) into output (UI).

```jsx
// Pure component example
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

## The State Management Challenge

While pure functions are great for predictability, real applications need to handle state changes. Consider this scenario from RPGMyLife:

```jsx
function Character({ name, level, experience }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Level: {level}</p>
      <p>XP: {experience}</p>
      <QuestLog quests={quests} />
      <Inventory items={items} />
    </div>
  );
}
```

When a player completes a quest, multiple components need to update:
- The experience counter increases
- The quest log updates
- Potentially new items appear in inventory
- The level might increase

## Three Approaches to State Management

### 1. Prop Drilling

The traditional approach passes state and updater functions through props:

```jsx
function App() {
  const [character, setCharacter] = useState(initialCharacter);
  
  return (
    <GameUI 
      character={character}
      onQuestComplete={updateCharacter}
    />
  );
}

function GameUI({ character, onQuestComplete }) {
  return (
    <QuestLog 
      quests={character.quests}
      onQuestComplete={onQuestComplete}
    />
  );
}

function QuestLog({ quests, onQuestComplete }) {
  return (
    <Quest 
      quest={quests[0]}
      onComplete={onQuestComplete}
    />
  );
}
```

**Problems:**
- Verbose
- Components need to pass props they don't use
- Refactoring becomes difficult

### 2. Redux (Flux Pattern)

Redux centralizes state management:

```jsx
// Action
const completeQuest = (questId) => ({
  type: 'COMPLETE_QUEST',
  payload: { questId }
});

// Reducer
function characterReducer(state, action) {
  switch (action.type) {
    case 'COMPLETE_QUEST':
      return {
        ...state,
        experience: state.experience + 100,
        quests: state.quests.filter(q => q.id !== action.payload.questId)
      };
    default:
      return state;
  }
}

// Component
function Quest({ quest }) {
  const dispatch = useDispatch();
  
  return (
    <button onClick={() => dispatch(completeQuest(quest.id))}>
      Complete Quest
    </button>
  );
}
```

**Benefits:**
- Centralized state management
- Predictable state updates
- Great for complex applications

**Drawbacks:**
- Additional boilerplate
- Learning curve
- Might be overkill for simpler apps

### 3. React Context

Context provides a middle ground:

```jsx
// Create the context
const CharacterContext = createContext();

// Provider component
function CharacterProvider({ children }) {
  const [character, setCharacter] = useState(initialCharacter);
  
  const completeQuest = useCallback((questId) => {
    setCharacter(prev => ({
      ...prev,
      experience: prev.experience + 100,
      quests: prev.quests.filter(q => q.id !== questId)
    }));
  }, []);

  return (
    <CharacterContext.Provider value={{ character, completeQuest }}>
      {children}
    </CharacterContext.Provider>
  );
}

// Using the context
function Quest({ quest }) {
  const { completeQuest } = useContext(CharacterContext);
  
  return (
    <button onClick={() => completeQuest(quest.id)}>
      Complete Quest
    </button>
  );
}
```

**Benefits:**
- No prop drilling
- Simpler than Redux
- Built into React
- Perfect for medium-complexity applications

## When to Use Each Approach

- **Prop Drilling**: Simple applications with shallow component trees
- **Context**: Medium-sized applications or sections of larger apps that need shared state
- **Redux**: Large applications with complex state interactions or when you need robust dev tools

## Real-World Example: RPGMyLife

In RPGMyLife, we chose Context for character state management because:
1. The state updates are relatively simple
2. Multiple components need access to character data
3. We wanted to avoid Redux complexity
4. The state is naturally hierarchical (character data flows down to quests, inventory, etc.)

Here's our actual implementation:

```jsx
// CharacterContext.tsx
export const CharacterContext = createContext<CharacterContextType>({
  character: null,
  updateExperience: () => {},
  completeQuest: () => {},
  addInventoryItem: () => {}
});

export function CharacterProvider({ children }) {
  // ... implementation details ...
}

// Usage in components
export function QuestLog() {
  const { character, completeQuest } = useContext(CharacterContext);
  
  if (!character) return null;
  
  return (
    <div className="quest-log">
      {character.quests.map(quest => (
        <Quest 
          key={quest.id}
          quest={quest}
          onComplete={completeQuest}
        />
      ))}
    </div>
  );
}
```

## Conclusion

React Context strikes an excellent balance between simplicity and power. While Redux remains valuable for complex applications, Context has become the go-to solution for many modern React applications. Understanding when to use each approach will help you make better architectural decisions in your React projects.

For RPGMyLife, Context provided exactly what we needed: a clean way to share state without overcomplicating our application architecture.