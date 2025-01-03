default_platform(:ios)

platform :ios do

  desc "Setup Project in Apple system"
  lane :register_app do
    produce(
      username: "appleid.email@gmail.com",
      app_identifier: "com.app.identifier",
      app_name: "AppName",
      team_name: "TeamName",
      itc_team_name: "AppstoreConnectTeamName"
    )
  end


  # To begin use match, it must be seted up with command - fastlane match init
  # To be sure that Matchfile updated

  desc "Sync Code-Signing assets"
  lane :sync_signing_assets do |options|
    selectedType = options[:type]
    match(type: selectedType)
  end

  desc "Register Devices on the Developer Portal"
  lane :sync_devices_info do
    register_devices(
      devices_file: "fastlane/Devicefile"
    )
  end

  desc "Update certificates and PP"
  lane :bootstrap_code_sign do
    sync_devices_info
    sync_signing_assets(type: "development")
    sync_signing_assets(type: "adhoc")
    sync_signing_assets(type: "appstore")
  end


  # To begin use gym, it must be seted up with command - fastlane gym init
  # To be sure that Gymfile updated

  desc "Build App Store build"
  lane :build_appstore do
    precheck_version
    sync_signing_assets(type: "appstore")
    increment_build_number
    gym(
      output_directory: "build_Appstore",
      export_method: "app-store"
    )
  end

  desc "Build Ad Hoc build"
  lane :build_adhoc do
    precheck_version
    sync_signing_assets(type: "adhoc")
    increment_build_number
    gym(
      output_directory: "build_AdHoc",
      export_method: "ad-hoc"
    )
  end

  desc "Distribute build to AppStore"
  lane :distribute_to_appstore do
    build_appstore
    pilot(
      team_name: "TeamName",
      changelog: "Version {lane_context[SharedValues::VERSION_NUMBER]}, Build {lane_context[SharedValues::BUILD_NUMBER]}
    )
  end

  desc "Check version for Appstore review with Precheck tool"
  lane :precheck_version do
    precheck
    UI.success "Version meet requirements and ready for review"
  end

end