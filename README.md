# swift-ui

# Documentazione :
https://developer.apple.com/tutorials/swiftui/creating-and-combining-views



# See This :
https://betterprogramming.pub/swiftui-basic-components-ac2c62dc7b95
https://www.hackingwithswift.com/quick-start/swiftui/displaying-a-detail-screen-with-navigationlink
https://www.simpleswiftguide.com/advanced-swiftui-button-styling-and-animation/

Command + SHIFT + L

Posso fare il drag and drop degli elementi UI

VStack -> significa vertical Stack


```
VStack {
    Text("Hello Word").padding()
    Button(action : action){
        content
    }
}
```

HStack -> Stack orizzontale


3 ) Tipi di Stack

Horizontal Stack
Vertical Stack
Z Stack


Implementiamo il Background:

ZStack {
    Color(.blue).endgesIgnoringSafeArea(.all)
}

Posso anche usare un LinearGradient



LinearGradient(gradient : Gradient(colors : [Color.red , Color.blue])),
    startPoint : .leading , endPoint : .trailing )
   .endgesIgnoringSafeArea(.all)

Nota : Colors è un vettore di colori ... colors :[.blue , .red , .white]


Text("Wooow Thats cool")
    .font(.system(size : 32 , weight : .medium , design : .default))
    .foregroundColor(.white)
    .background(Color.red)
    .frame(width : 200 , height : 200 )

Spacer()  <--- Prende l'intero spazio



# Intro a SF Symbols 2

SF Symbols 2
Sono icone che ho già a dispozione.

https://developer.apple.com/sf-symbols/

Esempio di come accedere a SF symbols :

```
VStack {
    Image(systemName : "cloud.sun.fill")
        .renderingMode(.original)
        .resizable()
        .aspectRatio(contentMode : .fit)
        .frame(width : 180 , height : 180 , alignment : .center)

    Text("Another Example")
        .font(.system(size : 70 , weight : .medium))
        .foregroundColor(.white)
}

VStack(spacing : 10 ) {
}<- Ogni elemento ha uno space di 10 unità
```



# Intro a come creare migliori UI

Command + click su VStack
Si apre un menu -> Selezionare Exstract SubView

Crea una funzione a parte.

```
struct ExampleOfView : View {

    var myName : String
    var imageName : String


    var body : some View {

        VStack{
            Text("Wow this is cool \(myName)")
        }

    }

}
```


Posso richiamamere il codice in qualsiasi
punto mi devo solamente ricordare che
devo passare myName , e myImage

ExampleOfView(myImage : "" , myName : "")

# How to make Buttons

```
Button {
    print("tapped")
} label :{
    Text("Click here")
        .frame(width : 200 , height : 50)
        .background(Color.white)
        .font(.system(size : 20 , weight : . bold , design : .default))
        .cornerRadius(10)
}
```


# Handle Image 

Image("nomeImmagine")
    .resizable()
    .aspectRadio(contentMode : .fit)
    .frame(width : 200 , height : 200 , aligment : .center )
    



# @State & @Binding


@State
@Binding
@StateObject
@ObservedObject
@EnviromentObject


# Example
```
@State private var isInDarkMode = false


Button {
    isInDarkMode.toggle()
}
The purpose of Toggle is simple: it is used to bind a property.
In some cases, we can use it to alter the screen, showing or hiding other views.

```



# Make Buttons with Gradients 

```
struct GradientButtonStyle: ButtonStyle {
    func makeBody(configuration: Self.Configuration) -> some View {
        configuration.label
            .foregroundColor(Color.white)
            .padding()
            .background(LinearGradient(gradient: Gradient(colors: [Color.red, Color.orange]), startPoint: .leading, endPoint: .trailing))
            .cornerRadius(15.0)
    }
}


Button(action: {
    print("Button action")
}) {
    HStack {
        Image(systemName: "bookmark.fill")
        Text("Bookmark")
    }
}.buttonStyle(GradientButtonStyle())

```



# Example of using the state

```
struct ContentView: View {
    @State private var users = ["Paul", "Taylor", "Adele"]

    var body: some View {
        NavigationView {
            List {
                ForEach(users, id: \.self) { user in
                    Text(user)
                }
                .onDelete(perform: delete)
            }
            .navigationTitle("Users")
        }
    }

    func delete(at offsets: IndexSet) {
        users.remove(atOffsets: offsets)
    }
}
```

# Binding

@Binding lets us declare that one value actually comes from elsewhere, and should be shared in both places.


```
struct AddView: View {
    @Binding var isPresented: Bool

    var body: some View {
        Button("Dismiss") {
            isPresented = false
        }
    }
}
```
That property literally means “I have a Boolean value called isPresented, but it’s being stored elsewhere.”
So, when we create that AddView to replace the // show the add user view comment from earlier, we’d need to provide the value so it can be manipulated:

```
.sheet(isPresented: $showingAddUser) {
    AddView(isPresented: $showingAddUser)
}
```


# Example of using ObservableObject ,  @Published and StateObject


When using observed objects there are three key things we need to work with: the ObservableObject protocol is used with some sort of class that can store data, the @ObservedObject property wrapper is used inside a view to store an observable object instance, and the @Published property wrapper is added to any properties inside an observed object that should cause views to update when they change.

Tip: It is really important that you use @ObservedObject only with views that were passed in from elsewhere. You should not use this property wrapper to create the initial instance of an observable object – that’s what @StateObject is for.

As an example, here’s a UserProgress class that conforms to ObservableObject:


```
class UserProgress: ObservableObject {
    @Published var score = 0
}

```

```
struct InnerView: View {
    @ObservedObject var progress: UserProgress

    var body: some View {
        Button("Increase Score") {
            progress.score += 1
        }
    }
}
```

```
struct ContentView: View {
    @StateObject var progress = UserProgress()

    var body: some View {
        VStack {
            Text("Your score is \(progress.score)")
            InnerView(progress: progress)
        }
    }
}

```










