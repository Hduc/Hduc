## Câu lệnh trong flutter 
```
Flutter build web
Flutter doctor
Flutter devices
flutter clean
```
### Get package in file pubspec.yaml
`Flutter pub get`

### Update package
`Flutter pub upgrade`

### Chuyển về môi trường
```
Flutter channel dev
Flutter channel stable
```
### Chạy  debugger trên  [edge, chrome, windows, macos, linux]
`Flutter run  -d <platform>`

### Where <platform> is windows, macos, or linux
`Flutter config --enable-<platform>-desktop`
  
### Tạo bản build trên source
`Flutter create --platforms=windows,macos,linux . `

### Fix run
#### "error running pod install" 
```
# Uninstall the local cocoapods gem
sudo gem uninstall cocoapods

# Reinstall cocoapods via Homebrew
brew install cocoapods
```
#### build web
`flutter build web --web-renderer canvaskit --no-tree-shake-icons`
