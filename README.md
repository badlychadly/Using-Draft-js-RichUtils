# Using-Draft-js-RichUtils

In my last post called [Starting with Draft-js](https://github.com/badlychadly/Starting-with-Draft-js/blob/master/README.md) we covered the basic setup to get up and running with Draft, and how to update SelectionState. In this post I want to cover using the RichUtils module. RichUtils enables users to use inline styling like Bold or Italic. RichUtils comes with core key commands such as ctrl+b makes bold, but we can go beyond that with our own function because of the handleKeyCommand prop on our Editor instance.

### Using RichUtils
To install RichUtils we just need to import it from the draft-js package.
```
import { Editor, EditorState, RichUtils} from 'draft-js';
```
Our next step is to declare the handleKeyCommand method.
```
handleKeyCommand = (command) => {
    const newState = RichUtils.handleKeyCommand(this.state.editorState, command)
    if (newState) {
        this.onChange(newState);
        return 'handled';
    }
    return 'not-handled';
}
```
This method takes a command as an argument and passes it to `RichUtils()` along with the editorState which returns a new EditorState, then the new EditorState gets passed to `onChange()`.
All that is left to do is pass this function as a prop to the Editor.
```
<Editor
    editorState={this.state.editorState}
    handleKeyCommand={this.handleKeyCommand}
    onChange={this.onChange}
/>
```
Now it will work in the Draft text editor. If you don't want to just push keys, you can add a button with an onClick event. The handler will use the RichUtils module again except call the method `toggleInlineStyle()` with the Editor and the Command as arguments.
```
onBoldClick = () => {
 this.onChange(RichUtils.toggleInlineStyle(this.state.editorState, 'BOLD'))
  }
```
You can go much further with RichUtils in terms of customization like using the `keyBindingFn` prop. I recommend you check out the [Draft-js docs](https://draftjs.org/docs/advanced-topics-key-bindings.html)
