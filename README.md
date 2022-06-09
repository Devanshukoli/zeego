![Frame 32](https://user-images.githubusercontent.com/13172299/172878122-8e27ccd8-d04e-431d-85fb-099cbe48457a.png)

Beautiful, native menus for React Native + Web, inspired by Radix UI.

## Installation

Each component is tree-shaken into its own package, but you only need to install `zeego`.

```sh
yarn add zeego
```

Add peer deps:

```sh
yarn add react-native-ios-context-menu react-native-popper
```

### Next.js

You need to add `zeego` to your `next-transpile-modules` in `next.config.js`.

### Expo

You need to use a custom development client, since `react-native-ios-context-menu` uses native code.

After installing, you'll need to rebuild your custom development client and app.

### Vanilla React Native

Run `pod install` in your `ios` folder.

## Philosophy

![Optimized](https://user-images.githubusercontent.com/13172299/172878161-7a184ae0-3a04-4b5f-9efe-2240444ed184.png)

1. Use the best API possible
2. Create the best experience for each platform, without concern for sharing styles
3. Everything ships unstyled

The API follows that of Radix UI closely.


## Example

For now, you should reference the [example in the repo](https://github.com/nandorojo/zeego/tree/master/examples/expo/src/App.tsx).

## Usage

```ts
import * as DropdownMenu from 'zeego/dropdown-menu'
```

See radix-ui's dropdown menu. It's really similar.

One difference is that `DropdownMenu.Item` needs a child `DropdownMenu.ItemTitle`, since React Native separates `Text` and `View` components.

```tsx
<DropdownMenu.Item>
  <DropdownMenu.ItemTitle>Bookmark</DropdownMenu.ItemTitle>
  <DropdownMenu.ItemIcon iosIconName="bookmark">
    <YourIconComponent />
  </DropdownMenu.ItemIcon>
</DropdownMenu.Item>
```

### Custom components

To use a custom component, you'll first need to `menuify` it.

Here is an example of a custom component using Dripsy:

```tsx
import * as DropdownMenu from 'zeego/dropdown-menu'
import { styled } from 'dripsy'

const StyledMenuItem = styled(DropdownMenu.Item)({
  height: 32,
})

// this part is important
const DripsyMenuItem = DropdownMenu.menuify(StyledMenuItem, 'Item')
```

And now, you can use it:

```tsx
<DripsyMenuItem key="fernando">
  <DropdownMenu.ItemTitle>Fernando</DropdownMenu.ItemTitle>
<DripsyMenuItem>
```

Under the hood, `menuify` applies a `displayName` to your component. This allows `zeego` to recognize it when it's mapping children for iOS and Android.

## Example

See the [Example app](https://github.com/nandorojo/zeego/blob/master/examples/expo/src/App.tsx).

```tsx
const DropdownMenuExample = () => {
  const [bookmarked, setBookmarked] = useState<'on' | 'off' | 'mixed'>('on')
  return (
    <DropdownMenu.Root>
      <DropdownMenu.Trigger>
        <View>
          <Text style={styles.button}>{`<DropdownMenu />`}</Text>
        </View>
      </DropdownMenu.Trigger>
      <DropdownMenu.Content style={dropdownStyles.content}>
        <DropdownMenuLabel>Help</DropdownMenuLabel>
        <DropdownMenuItem
          style={dropdownStyles.item}
          onSelect={select(1)}
          key="first"
        >
          <DropdownMenuItemTitle style={dropdownStyles.itemTitle}>
            See more
          </DropdownMenuItemTitle>
          {Platform.OS === 'ios' && (
            <DropdownMenu.ItemSubtitle style={dropdownStyles.itemSubtitle}>
              12 artists fit your search
            </DropdownMenu.ItemSubtitle>
          )}
          <DropdownMenuItemIcon iosIconName="list.star">
            <Ionicons name="list" size={15} />
          </DropdownMenuItemIcon>
        </DropdownMenuItem>
        <DropdownMenuItem
          style={dropdownStyles.item}
          onSelect={select(2)}
          key="second"
        >
          <DropdownMenuItemTitle>Favorite</DropdownMenuItemTitle>
          <DropdownMenuItemIcon iosIconName="star.fill">
            <Ionicons name="star" size={15} />
          </DropdownMenuItemIcon>
        </DropdownMenuItem>
        <DropdownMenuCheckboxItem
          style={dropdownStyles.item}
          value={bookmarked}
          onValueChange={setBookmarked}
          key="third"
        >
          <DropdownMenuItemIndicator>
            <Ionicons name="checkmark" size={19} />
          </DropdownMenuItemIndicator>
          <DropdownMenuItemTitle>
            {bookmarked === 'on' ? 'Bookmarked' : 'Bookmark'}
          </DropdownMenuItemTitle>
          <DropdownMenuItemImage
            iosIconName="book"
            source={require('./camera-outline.png')}
            width={20}
            resizeMode="contain"
          />
        </DropdownMenuCheckboxItem>
      </DropdownMenu.Content>
    </DropdownMenu.Root>
  )
}
```

## TODO

- [ ] `zeego/context-menu` (in-progress, missing Web implementation)
- [x] Android Support (currently a JS implementation)
- [ ] Docs
- [ ] `zeego/tooltip` (probably)
- [ ] `zeego/popover` (probably)
