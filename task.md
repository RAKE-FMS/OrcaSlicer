Currently the OrcaSlicer's OTA preset update check the the whole preset from the server, and compare it with the local preset. @src/slic3r/Utils/PresetUpdater.cpp.
In this new task, we want to only check the new version of the current vendor's preset.
The server will also has the changes that support checking only the new version of one specific vendor's preset. the server update will not be included in this task, but the client side will need to change the PROFILE_UPDATE_URL to support the new server API.
the server api requires vendor id and OrcaSlicer version.
The new PROFILE_UPDATE_URL will be https://check-version.orcaslicer.com/profile?vendor=vendor_name, and the response will be like this:
{
  "version": "1.2.3",
  "download_url": "https://check-version.orcaslicer.com/download/vendor_name/1.2.3"
}.

if there are no new version, the response will be 204 No Content.

With this update, we should achieve below goals:
1. When the user switches to a new vendor's preset(when swtiching to a new printer in sidebar), the OrcaSlicer will check if there is a new version of the preset for that vendor, and if there is, it will prompt the user to update the preset. The update workflow will be the same as the current workflow, but it will only update the preset for the current vendor, not all the presets.
2. User may switching printers frequently, the background check and update should handle it.
3. The changes should be narrowed and not overengineered.