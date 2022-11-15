# Electron IPC Main - Renderer 통신



## IPC Main.js

```javascript
import {ipcMain} from 'electron'
  
//ipcMain.handle('채널명', 리스너(이벤트,인자))
ipcMain.handle('loadFile', async()=>{
  ...
  
  return result
})
```



## preload.js

```javascript
import {contextBridge, ipcRenderer} from 'electron'

contextBridge.exposeInMainWorld('file',{
  loadFile(){
    return ipcRenderer.invoke('loadFile')
  }
})
```



## view.vue

```javascript
<script setup> 

const loadFile = async()=>{
  const loadFileData = await window.file.loadFile()
}
  
</script>
```

