---
title: React commonly used components
categories: React
tags: 
  - React
  - Productivity
  - Continued
excerpt: React components which always used by project
date: 2021-10-25 20:33:36
cover: 'https://pic2.zhimg.com/v2-58d51f508827b08c7eb2fec472f751a7_1440w.jpg'
---

## Component list

### react-split-pane

> Split-Pane React component, can be nested or split vertically or horizontally!

react-split-pane: [react-split-pane](https://github.com/tomkp/react-split-pane)

eg:

``` jsx
<SplitPane split="vertical" minSize={50} defaultSize={100}>
  <div />
  <div />
</SplitPane>
<SplitPane split="vertical" minSize={50}>
  <div />
  <SplitPane split="horizontal">
    <div />
    <div />
  </SplitPane>
</SplitPane>
```
### React ahooks

> React Hooks Library.

More info: [ahooks](https://github.com/alibaba/hooks)

### react-lines-ellipsis

> Poor man's multiline ellipsis component for React.JS https://xiaody.github.io/react-lines-ellipsis/

eg:
``` JSX
import LinesEllipsis from 'react-lines-ellipsis'

<LinesEllipsis
  text='long long text'
  maxLine='3'
  ellipsis='...'
  trimRight
  basedOn='letters'
/>

```
More info: [react-lines-ellipsis](https://github.com/xiaody/react-lines-ellipsis)

### 