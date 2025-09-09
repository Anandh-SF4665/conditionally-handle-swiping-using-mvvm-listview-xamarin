**[View document in Syncfusion Xamarin Knowledge base](https://www.syncfusion.com/kb/12267/how-to-conditionally-handle-the-swiping-using-mvvm-in-xamarin-forms-listview-sflistview)**

## Sample

```xaml
<syncfusion:SfListView x:Name="listView" ItemSpacing="1" AllowSwiping="True" AutoFitMode="Height" SelectionMode="None" ItemsSource="{Binding ContactsInfo}">
    <syncfusion:SfListView.ItemTemplate >
        <DataTemplate>
            <code>
            . . .
            . . .
            <code>
        </DataTemplate>
    </syncfusion:SfListView.ItemTemplate>

    <syncfusion:SfListView.LeftSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="Black">
                <Label Text="Left Swipe" TextColor="White" FontAttributes="Bold" VerticalTextAlignment="Center" HorizontalTextAlignment="Center"/>
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.LeftSwipeTemplate>

    <syncfusion:SfListView.RightSwipeTemplate>
        <DataTemplate>
            <Grid BackgroundColor="Black">
                <Label Text="Right Swipe" TextColor="White" FontAttributes="Bold" VerticalTextAlignment="Center" HorizontalTextAlignment="Center"/>
                <Grid.GestureRecognizers>
                    <TapGestureRecognizer Command="{Binding Source={RelativeSource AncestorType={x:Type local:ContactsViewModel}}, Path=TakePhotoCommand}" CommandParameter="{x:Reference listView}"/>
                </Grid.GestureRecognizers>
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.RightSwipeTemplate>
</syncfusion:SfListView>

ViewModel.cs:

public Command<object> SwipeStartedCommand { get; set; }

SwipeStartedCommand = new Command<object>(OnSwipeStarted);

private void OnSwipeStarted(object obj)
{
    var args = obj as Syncfusion.ListView.XForms.SwipeStartedEventArgs;
    if ((args.ItemData as Contacts).BackgroundColor == Color.LightGray)
    {
        args.Cancel = true;
    }
}

C#:

void OnEvent (object sender, object eventArgs)
{
    if (Command == null) {
        return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
        resolvedParameter = CommandParameter;
    } else if (Converter != null) {
        resolvedParameter = Converter.Convert (eventArgs, typeof(object), AssociatedObject, null);
    } else {
        resolvedParameter = eventArgs;
    }		

    if (Command.CanExecute (resolvedParameter)) {
        Command.Execute (resolvedParameter);
    }
}

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
    var behavior = (EventToCommandBehavior)bindable;
    if (behavior.AssociatedObject == null) {
        return;
    }

    string oldEventName = (string)oldValue;
    string newEventName = (string)newValue;

    behavior.DeregisterEvent (oldEventName);
    behavior.RegisterEvent (newEventName);
}
```
