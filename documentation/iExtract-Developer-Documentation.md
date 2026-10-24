# iExtract Developer Documentation

## Source Code

- **iExtract:** <https://github.com/Gustakev/iExtract>
- **Photo Captioning Component:** <https://github.com/BrendxnW/image-captioning-cnn-lstm>

## Directory Structure

- `iExtract.py`: The entry point for iExtract. Prompts the user to choose between the Standard CLI and the Textual Dashboard (Beta) at startup, then launches the selected interface.
- `README.md`: The README file for iExtract.
- `INSTALL.md`: End-user installation instructions.
- `SETUP.md`: System administrator / developer deployment and setup guide.
- `RELEASE_NOTES.md`: Release notes for the current version of iExtract.
- `LICENSE`: The Apache License 2.0, which governs this project.
- `NOTICE`: The credits (contributor names) that must be kept with any copy or modified version of iExtract.
- `requirements.txt`: A list of required modules that the user must install via `pip install -r requirements.txt`, or manually for each, in order to use iExtract.

- `/.github/workflows/`: Stores the workflow `.yml` file describing how continuous integration testing will be conducted.

- `/cli_components/`: Stores the code that runs the CLI and the Textual UI (TUI) for the application, accepting user interaction and calling logic from the `functional_components` directory to accomplish a given goal.

- `/documentation/`: Stores the user and developer documentation for iExtract.

- `/functional_components/`: Stores all backend logic and features of iExtract.
  - `/backup_locator_and_validator/`: Locates, validates, and parses an iPhone backup into the in-memory BackupModel.
    - `/app/`: Contains code relevant to the Application Layer of iExtract, the code that executes program logic, like value processing.
    - `/data/`: Contains code relevant to the Data Layer of iExtract, the code that reads/writes data.
    - `/domain/`: Contains the code relevant to the Domain Layer of iExtract, the data structures and definitions primarily utilized by this module.
  - `/conversion_engine/`: Converts proprietary media formats (HEIC → JPG, MOV → MP4) during extraction.
    - `/app/`: Contains code relevant to the Application Layer of iExtract, the code that executes program logic, like value processing.
    - `/data/`: Contains code relevant to the Data Layer of iExtract, the code that reads/writes data.
    - `/domain/`: Contains the code relevant to the Domain Layer of iExtract, the data structures and definitions primarily utilized by this module.
  - `/file_extraction_engine/`: Reconstructs album/collection folder structures from the backup onto local storage.
    - `/app/`: Contains code relevant to the Application Layer of iExtract, the code that executes program logic, like value processing.
    - `/data/`: Contains code relevant to the Data Layer of iExtract, the code that reads/writes data.
    - `/domain/`: Contains the code relevant to the Domain Layer of iExtract, the data structures and definitions primarily utilized by this module.
  - `/photo_caption/`: Beta feature: sends sample images to a remote CNN-LSTM model and returns a generated caption.
    - `/app/`: Contains code relevant to the Application Layer of iExtract, the code that executes program logic, like value processing.
    - `/data/`: Contains sample image sets used for the in-app Photo Descriptor demo.
  - `/sql_cmd_facilitator/`: Handles all SQLite database interactions against `Photos.sqlite` and `Manifest.db`.
    - `/app/`: Contains code relevant to the Application Layer of iExtract, the code that executes program logic, like value processing.
    - `/data/`: Contains code relevant to the Data Layer of iExtract, the code that reads/writes data.
    - `/domain/`: Contains the code relevant to the Domain Layer of iExtract, the data structures and definitions primarily utilized by this module.
  - `services.py`: Application Layer service classes that bridge the Presentation Layer with the functional components.
  - `iphone_models.json`: Maps internal Apple model identifiers to branded display names.
  - `directory-explanation.md`: An explanation of the directory structure.

- `/tests/`: Stores the test cases that are being used against the program.
  - `/test_data/`: Stores data for tests, if needed.

## How to Build the Software

Run the following steps from a fresh clone.

1. **Clone repo**
   1. In the terminal (using the shell of your choice) run the command:

      ```bash
      git clone https://github.com/Gustakev/iExtract.git
      ```

2. **Move to repo location**
   1. In the terminal, run the command: `cd iExtract`
3. **Install Python**
   1. Install Python 3.13+ for your system from this link: <https://www.python.org/downloads/>
4. **Initialize virtual environment**
   1. In the terminal, run the command: `python -m venv venv`
   2. Note: If the above command, or any other command prefixed with `python` fails, ensure that you have a Python path variable set on your system. It may either be `python`, `python3`, or something more specific like `python3.13`, depending on your installation.
5. **Activate virtual environment**
   1. If your machine is running Mac or Linux, in the terminal, run the command:

      ```bash
      source venv/bin/activate
      ```

   2. If you have a Windows machine, in administrator PowerShell, run the command: `.\venv\Scripts\Activate.ps1`
6. **Install requirements**
   1. In the terminal, run the command: `pip install -r requirements.txt`
   2. **Note:** Some Linux users may need to run the command: `sudo apt install python3-tk` in order to obtain a working installation of `tkinter`.
7. **How to deactivate the virtual environment**
   1. Enter `deactivate` into the terminal.

## How to Test the Software

*Online Continuous Integration*: In order to run the system's test cases, the automated system that we have enabled in GitHub via the use of GitHub Actions, utilizing `unittest` as our testing technology, a developer must simply follow the following steps:

1. Make a push to the repository containing iExtract (or make a pull request to that repository).
2. GitHub Actions will automatically run the test suite against the commit. Wait for it to finish.
3. View the results of the test in the GitHub Actions tab.

*Offline Testing:* Run the command `python -m unittest discover tests` locally from the root directory of the project repo.

### How to Add New Tests

Are there any naming conventions/patterns to follow when naming test files? Is there a particular test harness to use?

To add tests, from the project root, enter the `/tests/` directory, then create a test file adhering to the following requirements:

1. Must be testable via the command `python -m unittest discover tests`
2. Files must start with `test_`
3. Files must end with `.py`
4. **NOTE:** Any test data, like CSV files, for example, must go in the `test_data` folder within the `tests` folder.

*Example Test:* `test_backup_locator_and_validator.py`

*Conventions (Within Test Files):* Test cases will start each function with `test_` followed by the task.

## How to Build a Release of the Software

*Follow These Steps:*

1. Pull the latest changes from the main branch.
2. Run the test suite offline by entering the command `python -m unittest discover tests` into the terminal.
3. Confirm that the tests pass in GitHub Actions for this commit.
4. Launch iExtract to ensure that basic functionality isn't broken.
5. Update the version number of iExtract in the `README.md` file, under the following scheme:
   1. `major_version.minor_version.bug_fix` (X.Y.Z) versioning scheme, iterating the correct number corresponding to the update
6. Once the version number has been updated, tag the release/version number using Git:
   1. `git tag vX.Y.Z`
   2. `git push origin vX.Y.Z`
7. Create a GitHub release corresponding to the version you just tagged:
   1. On the project's repo page on GitHub, under 'Releases':
      1. Select 'Create a new release'
      2. Select the correct tag (the one you just made in *Step 6*)
      3. Add a release summary with a title and a description
      4. Attach relevant artifacts, such as a compressed directory containing the repository, along with executables for the 3 main targeted platforms: Windows 10/11 x64, Linux x64, and macOS Apple Silicon 64-bit
      5. Release to the correct branch
8. *Post-Release Sanity Checks:*
   1. Clone the repo to a fresh directory and ensure that the installation instructions actually work.
   2. Ensure that the release tag matches the one named in `README.md`.
   3. Ensure that the release notes are pertinent to the changes made.

## How to Package This Application

iExtract can be packaged into a standalone executable for Windows, Linux, and macOS using PyInstaller. Each platform must be built natively — cross-compilation is not supported. Use Python 3.13 specifically; pydantic has known compatibility issues with Python 3.14 during the PyInstaller build process.

### Windows 10/11 x64

1. Install Python 3.13 from python.org. During install, check "Add Python to PATH". Verify with: `py -3.13 --version`
2. In the project root, create and activate a dedicated build environment:

   ```powershell
   py -3.13 -m venv buildenv
   buildenv\Scripts\activate
   ```

3. Install dependencies:

   ```powershell
   pip install -r requirements.txt
   pip install pyinstaller
   ```

4. Build:

   ```powershell
   python -m PyInstaller --onefile --copy-metadata imageio --copy-metadata pillow_heif --copy-metadata charset_normalizer --add-data "functional_components/photo_caption/data;functional_components/photo_caption/data" --add-data "functional_components/iphone_models.json;functional_components" --add-data "cli_components/main_menu.tcss;cli_components" --hidden-import PIL --hidden-import PIL.Image --hidden-import pillow_heif --hidden-import tkinter --hidden-import charset_normalizer --collect-all imageio_ffmpeg --collect-all charset_normalizer iExtract.py
   ```

5. Output: `dist\iExtract.exe`. Note: symlink creation on Windows requires Administrator privileges or Developer Mode (Settings > System > For Developers). Without either, iExtract automatically falls back to copying files and will display a warning.

### Linux x64

1. Install system dependencies and Python 3.13 (pillow-heif requires libheif at the system level):

   ```bash
   sudo apt install libheif-dev python3.13 python3.13-venv
   ```

   - **Note:** Some Linux users may need to run the command: `sudo apt install python3-tk` in order to obtain a working installation of `tkinter`.

2. Create and activate a build environment, install dependencies, then build:

   ```bash
   python3 -m PyInstaller --onefile --copy-metadata imageio --copy-metadata pillow_heif --copy-metadata charset_normalizer --add-data "functional_components/photo_caption/data:functional_components/photo_caption/data" --add-data "functional_components/iphone_models.json:functional_components" --add-data "cli_components/main_menu.tcss:cli_components" --hidden-import PIL --hidden-import PIL.Image --hidden-import pillow_heif --hidden-import tkinter --hidden-import charset_normalizer --collect-all imageio_ffmpeg --collect-all charset_normalizer iExtract.py
   ```

3. Output: `dist/iExtract`. Run `chmod +x dist/iExtract` if needed.

### macOS Apple Silicon

1. Install Homebrew if not already present (brew.sh), then install system dependencies:

   ```bash
   brew install libheif python@3.13
   ```

2. Create and activate a build environment, install dependencies, then build:

   ```bash
   python3 -m PyInstaller --onefile --copy-metadata imageio --copy-metadata pillow_heif --copy-metadata charset_normalizer --add-data "functional_components/photo_caption/data:functional_components/photo_caption/data" --add-data "functional_components/iphone_models.json:functional_components" --add-data "cli_components/main_menu.tcss:cli_components" --hidden-import PIL --hidden-import PIL.Image --hidden-import pillow_heif --hidden-import tkinter --hidden-import charset_normalizer --collect-all imageio_ffmpeg --collect-all charset_normalizer --target-arch arm64 iExtract.py
   ```

3. Output: `dist/iExtract`. Run `chmod +x dist/iExtract` if needed. Note: macOS may show a Gatekeeper warning on first run since the binary is unsigned. Right-click > Open > Open anyway, or run directly from the terminal.
