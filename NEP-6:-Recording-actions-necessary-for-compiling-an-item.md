**Status: Implemented.**

A “recorder” object could record all filter and layout calls for an item in a compilation rule. The recorded calls could then be stored; when the item is recompiled again, the recorder will be used first, and if the recorded actions match the stored actions, the item does not need to be recompiled but the cached, already compiled content can be reused.