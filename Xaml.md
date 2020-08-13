# 基本

## 名称空间

```xml
<Window 
        x:Class="WpfApp3.MainWindow" //所属类,标识窗口对应的代码不在一个单独的文件中
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"//WPF默认名称空间
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"//
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"//blend
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp3"//local所在的名称空间
        xmlns:sys="clr-namespace:System;assembly=mscorlib"//系统名称空间
        mc:Ignorable="d"//可忽略的名称空间
        Title="MainWindow" Height="450" Width="800"/>

```

## 路由事件

标准事件：显示订阅事件处理代码，并发送到订阅者

路由事件：事件发生后会向该事件控件的上层与下层空间传递，
向上传递被称为冒泡事件（bubbling event)，通常为事件向父元素传递
向下传递被称为下钻事件（tunneling event），通常指从根元素传往执行操作的控件。下钻事件通常因加上preview前缀，并且总是在相应的冒泡事件前发生。
若要阻止传递，则将RoutedEventArgs的handled属性设置为true

```xml
<Window 
        x:Class="WpfApp3.MainWindow" 
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp3"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        mc:Ignorable="d"
        Title="MainWindow" Height="350" Width="525" KeyDown="Window_KeyDown"
        PreviewKeyDown="Window_PreviewKeyDown">
    <Grid KeyDown="Grid_KeyDown" PreviewKeyDown="Grid_PreviewKeyDown">
        <Button Content="Button" HorizontalAlignment="Left" Margin="221,115,0,0"
                VerticalAlignment="Top" Width="75"/>
        <Button x:Name="rotatedButton" Content="2nd Button" Width="75" Height="22"
                FontWeight="Bold" RenderTransformOrigin="0.5,0.5" KeyDown="rotatedButton_KeyDown"
                PreviewKeyDown="rotatedButton_PreviewKeyDown">
            <Button.RenderTransform>
                <TransformGroup>
                    <ScaleTransform/>
                    <SkewTransform/>
                    <RotateTransform Angle="-32.744"/>
                    <TranslateTransform/>
                </TransformGroup>
            </Button.RenderTransform>
        </Button>
    </Grid>
</Window>
```

```c#
public MainWindow() {
    InitializeComponent();
}

private void Grid_KeyDown(object sender, KeyEventArgs e) {
    MessageBox.Show("Grid hanlder, bubbling up");
}

private void Grid_PreviewKeyDown(object sender, KeyEventArgs e) {
    MessageBox.Show("Grid handler, tunneling down");
}

private void rotatedButton_KeyDown(object sender, KeyEventArgs e) {
    MessageBox.Show("rotatedButton handler, bubbling up");
}

private void rotatedButton_PreviewKeyDown(object sender, KeyEventArgs e) {
    MessageBox.Show("rotatedButton handler, tunneling down");
}
private void Window_KeyDown(object sender, KeyEventArgs e) {
    MessageBox.Show("Window handler, bubbling up");
}
private void Window_PreviewKeyDown(object sender, KeyEventArgs e) {
    MessageBox.Show("Window handler, tunneling down");
}
```

按除空格和回车键的任意键执行后，顺序如下

Window handler, tunneling down

Grid handler, tunneling down

rotatedButton handler, tunneling down

rotatedButton handler, bubbling up

Grid handler, bubbling up

Window handler, bubbling up

## 路由命令

当代码相应多个位置的操作时使用，例如使用save保存或使用下拉菜单保存。

尽管可以使用其他方法，但是通常会导致编辑重复代码

此外可以设置是否允许用户使用该命令

## 依赖属性

继承自System.Windows.DependencyObject的类可以注册依赖属性
需要添加public static修饰符，并且类型为DependencyProperty 
依照命名规范，依赖属性命名为\<PropertyName\>Property
使用DependencyProperty.Register()方法配置依赖属性，传递3-5个参数，参数对应如下

| 参数                                                   | 用法                                                         |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| **string** *name*                                      | 属性名称                                                     |
| **Type** *propertyType*                                | 属性类型                                                     |
| **Type** *ownerType*                                   | 包含的属性的类的类型                                         |
| **PropertyMetadata**<br />*typeMetadata*               | 额外的属性设置：属性的默认值，以及在属性变更通知和强制类型转换时用到的回调方法 |
| **ValidateValueCallBack**<br />*validateValueCallback* | 用于验证属性值的回调方法                                     |

 第四个参数应当继承于PropertyMetadata，例如FrameworkPropertyMetadata

```c#
using System.Windows;

class TestProperty : DependencyObject{
    //添加依赖属性
    public static DependencyProperty AStringProperty = DependencyProperty.Register(
    		"AString",
    		typeof(string),
    		typeof(TestProperty),
    		new FrameworkPropertyMetadata("Default Value"));
    //添加一个普通属性用于访问依赖属性
    public string AString{
        get{
        	return (string)GetValue(AStringProperty);
    	}set{
            SetValue(AStringProperty, value);
        }
    }
}
```

### 值转换器

以一个转换扑克牌面值的转换器为例

```c#
[ValueConversion(typeof(Rank), typeof(string))]
class RankNameConverter : IValueConverter{
    public object Convert(object, Type targetType, object parameter
                         System.Globalization.CultureInfo culture){
        int source = (int)value;
        if(source == 1 || source > 10){
            switch (source){
                case 1:
                    return "ACE";
                case 11:
                    return "Jack";
                case 12:
                    return "Queen";
                case 13:
                    return "King";
                default:
                    return DependencyProperty.UnsetValue;
            } else {
                return source.ToString();
            }
        }
    }
    public object ConvertBack(object value, Type targetType, object parameter,
                              System.Globalization.CultureInfo culture){
        return DependencyProperty.UnsetValue;
    }
}
```



## 事件注册

假设在UserControl中有一个按钮，在按下的时候需要触发BtnAClick事件，假设在UserControl中，该按钮引发BtnA_Click事件

```c#
class TestControl : UserControl{
    public event RoutedEventHandler BtnA{
        Add{
            AddHanlder(BtnAClickEvent,value);
        }
        Remove{
            RemoveHandler(BtnAClickEvent,value);
        }
    }

    public static readonly RoutedEvent BtnAClickEvent = 
        EventManager.RegisterRoutedEvent(
        "BtnAClick",
        RoutingStrategy.Bubble,
        typeof(RoutedEventHanlder),
    	typeof(TestControl));
    
    private void BtnA_Click(object sender, RoutedEventArgs e){
        RoutedEventArgs args = new RoutedEventArgs(BtnAClickEvent,this);
        RaiseEvent(args);
    }
}

```

