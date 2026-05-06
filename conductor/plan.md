# Avatar Upload & Cropping Implementation Plan

## Objective
Implement an avatar upload and cropping feature in `Avatar.vue` using the existing `vue-cropper` dependency. The cropped image will be converted to a Base64 data URL and passed back to the parent component.

## Key Files & Context
- `src/components/Avatar.vue`: Modal needs to be expanded to include an image file selector and the `VueCropper` component.
- `src/components/Prospect.vue`: Needs to listen to the new event to update its state.

## Implementation Steps

1. **Update `src/components/Avatar.vue`:**
    - Import `VueCropper` from `vue-cropper` and its CSS if necessary.
    - Define emits for `update:src`.
    - Add a hidden `<input type="file" accept="image/*">` and a button to trigger it.
    - Use `FileReader` to load the selected file as a base64 string and store it in a ref.
    - When an image is loaded, display the `VueCropper` component, binding its `img` prop to the loaded base64 string.
    - Add a template ref to the `VueCropper` component to access its methods.
    - In the "Confirm" handler, call the cropper's `getCropData` method to retrieve the cropped image as a Base64 string.
    - Emit the `update:src` event with the new Base64 string and close the modal.
    - Reset the cropper state when the modal closes (e.g. on "Cancel").

2. **Update `src/components/Prospect.vue`:**
    - Modify the `<Avatar :src="item.avatar" />` usage to listen for updates: `<Avatar :src="item.avatar" @update:src="(val) => item.avatar = val" />`.

## Verification & Testing
- Click the avatar in the UI to open the modal.
- Upload a local image file.
- Verify the cropper appears and allows adjusting the crop box.
- Click Confirm and verify the avatar updates in the UI.
- Reload the page to ensure the `Prospect.vue` localStorage persistence correctly saves the new base64 string.