# Stack: Introduction into screen layouts

Stack uses [XAML](https://docs.microsoft.com/en-us/dotnet/desktop-wpf/fundamentals/xaml)
for screen layouts. It is a technology, that Microsoft
developed to define user interfaces in 2010.

## Concepts

Stack is build around screen **layouts**. **Layout** is a text file, that
defines how to cut screen into pieces, and what should be in those pieces.

The most important thing you can put in a piece of a screen layout is a **Zone**.
**Zone** is an area of the screen, where windows can be placed. Typically,
every window fills the entire **Zone** it is added to, but you can also tell
Stack to stack windows in the **Zone** vertically or horizontally.

The [version of Stack sold in Windows Store](https://www.microsoft.com/en-us/p/stack-window-manager/9p4rj8rl7qgs) also supports **Tabs** element, which you can place anywhere on
the screen, and it will display all the windows in a **Zone** or multiple zones
like the browser tabs do.

In addition to that you can place many other stuff on your desktop via
a **layout**, like images, videos, or even simple
[3D scenes](https://docs.microsoft.com/en-us/dotnet/framework/wpf/graphics-multimedia/how-to-create-a-3-d-scene). Windows Store version also provides widgets and
various ways to display data from the Internet. See more information in
[widgets documentation](https://github.com/losttech/Stack.Widgets/blob/master/README.md).

## Basic layout

We recommend [WPF Tutorial](https://www.wpftutorial.net/LayoutProperties.html)
to get basics about how can you divide screen into parts, and put zones into
them. Start with one of the out of box layouts to get a hang of how things work
in practice (remember to make a copy: all out of box layouts are overwritten
after each app update!).

## Zones

This is an example of a **Zone** in a **layout**:

```XML
<zones:Zone x:Name="LargeZone" Id="MyLargeZone"/>
```

### Zone overlapping

Zones in Stack can overlap arbitrarily. A common scenario is to have a large
**Zone**, that has two or more *subzones*. When you drag a window around, you
want to be able to place it in either one of the subzones, or in the large
**Zone**.

In order to achieve that, out of box layouts typically put the large
**Zone** on the screen first, and then cover it with subzones. That way you can
easily drop the window in one of the subzones.

To be able to drop a window in the large **Zone** however, one has to define
some area above the subzones, so that if you leave the window there, it will
expand to the large **Zone**. You can do that in your layout by creating a
**drop Zone**, and setting its `Target` to the large **Zone** like this:

```XML
<zones:Zone x:Name="MyDropZone"
            Target="{Binding ElementName=LargeZone}"/>
```

When you drop your windows on `MyDropZone` they will end up on `LargeZone`
instead.

## Tabs

Tabs display can be placed anywhere in the layout, and show list of all windows in
a zone or multiple zones. You can have many tab displays on your layout, and
the same different tab lists can show the same window if necessary.

![Tabs demo](https://losttech.software/img/Stack-Tabs.gif)

When adding tabs to the layout, you just need to specify the source for the list
of windows via `ItemsSource` like this:

```xml
<zones:WindowTabs VisibilityCondition="AlwaysVisible"
    ItemsSource="{Binding Windows, Source={x:Reference YourZoneName}}" />
```

Here, `VisibilityCondition` can have one of the following values:

- **MultipleItems** (default) - only appear when there are multiple windows.
- AlwaysVisible - tabs are always visible (and take screen space).
- OneItem - tabs appear only when there's at least one window open.

## Dynamic layout: data binding and triggers

XAML, the language of Stack layouts, permits [**data binding**](https://docs.microsoft.com/en-us/dotnet/desktop-wpf/data/data-binding-overview) and [**triggers**](https://www.wpf-tutorial.com/styles/trigger-datatrigger-event-trigger/).
That means, that you can tell layout elements to change how they look like
depending on some conditions, like number of windows open, which one
is active, where your mouse is, etc. You can even use external conditions,
like your local weather or current price of some stock through our
[widgets library](https://github.com/losttech/Stack.Widgets/blob/master/README.md).

For the list of things, that you can bind to in Stack (in addition to the
standard things in XAML), check out [Data you can bind to](DataToBind.md).

## Extras

Please, see [What's New](https://losttech.software/stack-whatsnew.html) for the
extra features, that have been added recently, and might not have been
described here.

Also, check out [our blog](http://stack.blogs.losttech.software/) for some cool
stuff we made using Stack layouts.
