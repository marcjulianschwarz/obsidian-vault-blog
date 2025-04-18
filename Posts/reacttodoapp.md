---
blog-title: Wrote this Keyboard-Centric Todo App with React - What did I learn?
blog-tags:
  - EN
  - React
  - TypeScript
  - Web
blog-published: 2023-07-10
blog-archived: true
---
Hey there, in this post I want to share a side project of mine that I have been working on for a few days – a minimalist, *keyboard-centric* todo app. While showcasing the main features, I will refer you to some of the React resources and concepts that I used during the development of the app.
## Landing Page - Creating Your First Todo List

When you first open the app, you are presented with a rather minimalist-looking landing page. You can see the main keyboard shortcuts, and simply typing a name will create your very first todo list. 

![Start Page of the todo app](/images/todo_start.jpg)

> Implementing all of the keyboard shortcuts and navigation throughout the app taught me a lot about reference management via the [useRef React hook](https://react.dev/reference/react/useRef#manipulating-the-dom-with-a-ref) to control the focus of various HTML elements through the DOM.

The user interface was intentionally kept simple, to feel just like a piece of paper sitting right there on your desktop, waiting for you to jot down todos. Striking them through with a single keyboard click should feel as seamless and delightful as possible. That's why all of your actions will trigger the dynamic HUD to give you instant feedback.

![](/images/todo_list_done.jpg)

![](/images/todo_hud.jpg)


> Implementing the HUD made me use and learn a surprising amount of new React concepts. First, I created a [custom React Hook](https://react.dev/reference/react/cloneElement#extracting-logic-into-a-custom-hook) to abstract away the logic of showing and hiding the HUD element. Then, I used a [React portal](https://react.dev/reference/react-dom/createPortal#rendering-a-modal-dialog-with-a-portal) to correctly position it above all other content of the app. Lastly, I used the Publish-Subscribe Pattern to implement app-wide messaging via a [React context and context provider](https://react.dev/learn/passing-data-deeply-with-context) to facilitate the necessary methods.

## When One List Is Not Enough

Of course, a todo app would be nothing without the ability to create more than one list. None of your brilliant ideas will get lost again; just add them to your second "Ideas" list.

![](/images/todo_two_list.jpg)

But I didn't stop there. You can go crazy and add as many lists as you please. Productivity has never been this easy before 😉.

![](/images/todo_four_list.jpg)

## And Lastly, For All You Productivity Nerds (Including Myself)

It's 2023, our apps *need* command bars. 

![](/images/todo_cmd.jpg)

The command bar let's you create new lists, delete the ones you don't need anymore, and toggle the currently selected todo item. 

> Implementing the command bar made me use the [Reducer Pattern](https://kentcdodds.com/blog/the-state-reducer-pattern-with-react-hooks) (also see [useReducer](https://react.dev/reference/react/useReducer)) so that actions could be fired from everywhere within the app. These actions then trigger all of the backend operations (in this case local storage). Also UI changes are performed, such as showing the already mentioned HUD.

## Conclusion

Overall, this project taught me a lot about the various interesting concepts in the React framework. I had to build reusable components, write lots of CSS and think about ways to communicate between parts of the app. 
So, quite a success. 
