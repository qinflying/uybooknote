### 1.官方序列化接口 ISerializationCallbackReceiver

实现接口的2个方法：

(1)序列化前回调方法：OnBeforeSerialize

(2)序列化后回调方法：OnAfterSerialize

```c#
//示例
using System;
using UnityEngine;

[Serializable]
public class UIPanelInformation : ISerializationCallbackReceiver {
    [NonSerialized]
    public UIPanelType panelType;
    public string panelTypeString;
    public string path;

    public void OnAfterDeserialize() {
    //反序列化之后, 将一个或多个枚举字符串表示(panelTypeString)
    //转换成等效的枚举对象(UIPanelType)。
        UIPanelType type = (UIPanelType)Enum.Parse(typeof(UIPanelType),
        	panelTypeString);
        panelType = type;
    }
    public void OnBeforeSerialize() {
        //序列化前操作回调？可将不能序列化对象转换层序列化对象
    }
}
```



Dictionary的序列化泛型解决方案：

```c#
public class SerializationDictionary<TKey, TValue> : ISerializationCallbackReceiver
{
    [SerializeField]
    private List<TKey> keys;
    [SerializeField]
    private List<TValue> values;

    private Dictionary<TKey, TValue> target;
    public Dictionary<TKey, TValue> ToDictionary() { return target; }

    public SerializationDictionary(Dictionary<TKey, TValue> target)
    {
        this.target = target;
    }

    public void OnBeforeSerialize()
    {
        keys = new List<TKey>(target.Keys);
        values = new List<TValue>(target.Values);
    }

    public void OnAfterDeserialize()
    {
        var count = Math.Min(keys.Count, values.Count);
        target = new Dictionary<TKey, TValue>(count);
        for (var i = 0; i < count; ++i)
        {
            target.Add(keys[i], values[i]);
        }
    }
}
```

