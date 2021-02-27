---
title: WWDC20 Dub Dub Diary, Part 5 - Data Essentials in SwiftUI
layout: post
categories: WWDC
---

The last day of WWDC20, ended well with many amazing sessions. This day had an interesting session, which many of developers waited. For the last article of WWDC20, let's see the details and my last thoughts.

<br>

# Data Essentials in SwifUI

This session talks about how to deal data in SwiftUI. How to design your model, and techniques for your app. They explain three key questions when starting a new view in SwiftUI, which in this session answers all that questions.

- What data does this view need to do its job?
- How will the view manipulate that data?
- Where will the data come from?

## @State, @Binding

While creating a view, we need to know what data the view needs to render. Does the view displays the data or manipulate the data? If you are just showing the data, use `let` properties.

```swift
struct BookCard : View {
    let book: Book
    let progress: Double

    var body: some View {
        HStack {
            Cover(book.coverName)
            VStack(alignment: .leading) {
                TitleText(book.title)
                AuthorText(book.author)
            }
            Spacer()
            RingProgressView(value: progress)              
        }
    }
}
```

What if you are manipulating the data? Use `@State` and `@Binding` to preserve and seamlessly update the *Source of Truth*. `@State` creates a new Source of Truth, and `@Binding` is for sharing right access to any *Source of Truth*.

```swift
struct EditorConfig {
    var isEditorPresented = false
    var note = ""
    var progress: Double = 0
    mutating func present(initialProgress: Double) {
        progress = initialProgress
        note = ""
        isEditorPresented = true
    }
}

struct BookView: View {
    @State private var editorConfig = EditorConfig()
    func presentEditor() { editorConfig.present(…) }
    var body: some View {
        …
        Button(action: presentEditor) { … }
        ProgressEditor(editorConfig: $editorConfig)
        …
    }
}

struct ProgressEditor: View {
    @Binding var editorConfig: EditorConfig
    …
        TextEditor($editorConfig.note)
    …
}
```

- Properties for data that isn't mutated by the view
- `@State` for transient data owned by the view
- `@Binding` for mutating data owned by another view

## ObservableObject, @Published, @StateObject, @EnvironmentObject

```swift
/// The current reading progress for a specific book.
class CurrentlyReading: ObservableObject {
    let book: Book
    @Published var progress = ReadingProgress()
    @Published var isFinished = false

    var currentProgress: Double {
        isFinished ? 1.0 : progress.progress
    }
}

struct ReadingProgress {
    struct Entry : Identifiable {
        let id: UUID
        let progress: Double
        let time: Date
        let note: String?
    }

    var entries: [Entry]
}

struct BookView: View {
    @ObservedObject var currentlyReading: CurrentlyReading

    var body: some View {
        VStack {
            BookCard(
                currentlyReading: currentlyReading)

            HStack {
                Button(action: presentEditor) { /* … */ }
                    .disabled(currentlyReading.isFinished)

                Toggle(
                    isOn: $currentlyReading.isFinished
                ) {
                    Label(
                        "I'm Done",
                        systemImage: "checkmark.circle.fill")
                }
            }
            //…
        }
    }
}
```

`ObservableObject` lets you connect your views to your data model. It is a data dependency surface for SwiftUI's `View`s.

- Manage life cycle of your data
- Handle side-effects
- Integrate with existing components

`@Published` is used on to the property of the type that conforms to ObservableObject, which SwiftUI will be notified of their changes.

- Automatically works with `ObservableObject`
- Publishes every time the value changes in `willSet`
- `projectedValue` is a `Publisher`

They are three types of `ObservableObject`, and I recommend seeing the documentation for more information.

- `@ObservedObject` creates a data dependency
- `@StateObject` ties an `ObservableObject` to a view's life cycle
- `@EnvironmentObject` adds ergonomics to access `ObservableObject`

[State and Data Flow - Apple Developer Documentation](https://developer.apple.com/documentation/swiftui/state-and-data-flow){:target="_blank"}

## Source of Truth Lifetime

There are two property wrapper that can automatically save and restore *State*. Both are lightweight Storage you use in conjunction with your model.

`@SceneStorage` is a per-Scene scoped property wrapper. SwiftUI manage to read and write data. It is only accessible from within `View`s.

`@AppStorage` is a app-scoped global storage with is persisted using user defaults. It is usable anywhere, you can access it from within your `App` or from your `View`.

```swift
// SceneStorage
struct ReadingListViewer: View {
    @SceneStorage("selection") var selection: String?

    var body: some View {
        NavigationView {
            ReadingList(selection: $selection)
            BookDetailPlaceholder()
        }
    }
}

// AppStorage
struct BookClubSettings: View {
    @AppStorage("updateArtwork") private var updateArtwork = true
    @AppStorage("syncProgress") private var syncProgress = true

    var body: some View {
        Form {
            Toggle(isOn: $updateArtwork) {
                //...
            }

            Toggle(isOn: $syncProgress) {
                //...
            }
        }
    }
}
```

**Performance Tips**

- Make `View` initialization cheap
- Make `body` a pure function
- Avoid assumptions

[Data Essentials in SwiftUI - WWDC 2020 - Videos - Apple Developer](https://developer.apple.com/videos/play/wwdc2020/10040/){:target="_blank"}

<br>

# Wrap Up

This WWDC20 was all new, interesting experience. Meeting so many engineers, new code-along sessions, new online labs, letting all the developers watch the session without getting the tickets. It was also my first remote conference, watching developers communicate online made the conference more connected. Although, I hope Apple runs WWDC21 in person.