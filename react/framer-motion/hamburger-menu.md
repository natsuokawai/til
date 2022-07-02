ハンバーガーメニューをクリックしたらサイドバーが左から出てくるようにする

## セットアップ
```sh
npx create-react-app . --template=typescript
npm i styled-componets
npm i --save-dev @types/styled-components
npm i framer-motion
```

## 基礎
### motionコンポーネント
`motion.div`のように、HTMLやSVG要素に対応するmotionコンポーネントが用意されており、コンポーネントに対してアニメーションを付ける場合はこのコンポーネントを使用する。

今回使うpropsは以下の通り。
- initial
- animate
- variants
- transition

その他のprops: https://www.framer.com/docs/component/#props

### initial
コンポーネントの初期状態を規定する。

```js
// 初期状態は水平方向（X座標）に関して移動していない状態
<motion.div initial={{x: 0}} />
```

アニメーションの状態ではなく、variants（後述）のlabelを指定することもできる。

### animate
アニメーションの結果の状態。

```js
// 水平方向に関して左に300ピクセル移動する
<motion.div initial={{x: 0}} animate={{x: -300}} />
```

同じくvariantsのlabelを指定できる。

### variants
アニメーションの状態を定義するオブジェクト。状態名をプロパティとして、その状態名に対応する設定を値として指定する。

```js
// 例
const variants = {
  visible: { x: 0 },
  hidden: { x: -300 },
}

<motion.div initial='hidden' animate='visible' />
```

variantsを定義することで、アニメーションの状態をlabelで指定できる。

### transition
アニメーションの状態変化に関する設定。
https://www.framer.com/docs/transition/

今回、サイドバーは初期状態では隠れていて、ハンバーガーメニューをクリックしたら現れるようにしたいので、上記の設定値は以下のようになる。

- variants: `{ visible: { x: 0 }, hidden: { x: -width } }`
- initial: `hidden`
- animate: `isOpen ? 'visible' : 'hidden'`
- transition: `{ { type: 'spring', damping: 40, stiffness: 400 } }`

※サイドバーの幅を`width`とする。また、サイドバーが開いているかどうかの状態を`isOpen`で管理しているとする。

## コード
上記設定を実際に適用したコンポーネントは以下のようになる。

```tsx
import React, { useState } from 'react';
import styled from 'styled-components';
import { motion } from "framer-motion"

export const Sidebar = ({ width }: Props) => {
  const [isOpen, setOpen] = useState(false);
  return (
    <Container>
      <MenuButton
        onClick={() => setOpen(!isOpen)}
      >≡</MenuButton>
      <SidebarContainer
        width={width}
        initial={{ x: -width }}
        animate={isOpen ? 'show' : 'hide'}
        variants={{
          show: { x: 0 },
          hide: { x: -width },
        }}
        transition={{ type: 'spring', damping: 40, stiffness: 400 }}
      >
        サイドバーの中身
      </SidebarContainer>
    </Container>
  )
}

const Container = styled.div``

const MenuButton = styled.div`
  position: fixed;
  padding: 8px;
  top: 0;
  cursor: pointer;
  z-index: 3;
`

const SidebarContainer = styled(motion.div)<{width: number}>`
  position: fixed;
  top: 0;
  width: ${({width}) => width}px;
  height: 100%;
  background-color: white;
  z-index: 2;
`
```
