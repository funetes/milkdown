# React

We provide react support out of box.

## Install the Dependencies

Except the `@milkdown/core`, preset and theme.
We need to install the `@milkdown/react`,
which provide lots of abilities for react in milkdown.

```bash
# install with npm
npm install @milkdown/react @milkdown/core

# optional
npm install @milkdown/preset-commonmark @milkdown/theme-nord
```

## Create a Component

Create a component is pretty easy.

```typescript
import React from 'react';
import { Editor } from '@milkdown/core';
import { ReactEditor, useEditor } from '@milkdown/react';
import { commonmark } from '@milkdown/preset-commonmark';

import '@milkdown/theme-nord/lib/theme.css';

export const MilkdownEditor: React.FC = () => {
    const editor = useEditor((root) => new Editor({ root }).use(commonmark));

    return <ReactEditor editor={editor} />;
};
```

## Custom Component for Node

We provide custom node support out of box.

```typescript
import React from 'react';
import { Editor } from '@milkdown/core';
import { ReactEditor, useEditor, useNodeCtx } from '@milkdown/react';
import { commonmark, Paragraph, Image } from '@milkdown/preset-commonmark';

const CustomParagraph: React.FC = ({ children }) => <div className="react-paragraph">{children}</div>;

const CustomImage: React.FC = ({ children }) => {
    const { node } = useNodeCtx();

    return (
        <img
            className="react-image"
            src={node.attrs.src}
            alt={node.attrs.alt}
            title={node.attrs.tittle}
        />;
    )
};

export const MilkdownEditor: React.FC = () => {
    const editor = useEditor((root, renderReact) => {
        const nodes = commonmark
            .configure(Paragraph, { view: renderReact(CustomParagraph) })
            .configure(Image, { view: renderReact(CustomImage) });

        return new Editor({ root }).use(nodes);
    });

    return <ReactEditor editor={editor} />;
};
```

Values can be get by `useNodeCtx`:

-   _editor_:

    Instance of current milkdown editor.

-   _node_:

    Current prosemirror node need to be rendered.
    Equal to [node parameter in nodeViews](https://prosemirror.net/docs/ref/#view.EditorProps.nodeViews).

-   _view_:

    Current prosemirror editor view.
    Equal to [view parameter in nodeViews](https://prosemirror.net/docs/ref/#view.EditorProps.nodeViews).

-   _getPos_:

    Method to get position of current prosemirror node.
    Equal to [getPos parameter in nodeViews](https://prosemirror.net/docs/ref/#view.EditorProps.nodeViews).

-   _decorations_:

    Decorations of current prosemirror node.
    Equal to [decorations parameter in nodeViews](https://prosemirror.net/docs/ref/#view.EditorProps.nodeViews).
