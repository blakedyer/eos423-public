# lectures_readme

Some guidelines for using [RISE](https://github.com/damianavila/RISE), reveal.js, and [decktape](https://github.com/astefanutti/decktape).

Once RISE is installed, you can save your presentation as reveal.js slides directly from jupyter notebooks (listed under export). The .html file generated will need to be opened in a web browser (e.g. Chrome) from a directory that contains any included files in the presentation. For example, if the presentation has image slides stored in the subdirectory ''/images/'', you will need to make sure the 'output.html' file can see that directory.

To enable chalkboard in RISE presentations you must modify the notebook metadata (from the notebook top menu click Edit, then Edit Notebook Metadata). Add the following dictionary -- `"enable_chalkboard": true` is the most relevant line:  

    "rise": {
        "autoSlide": 5000,
        "chalkboard": {
          "chalkEffect": 1,
          "chalkWidth": 7,
          "color": [
            "rgb(34,47,62)",
            "rgb(34,47,62)"
          ],
          "readOnly": false,
          "src": "intro_python.json",
          "theme": "whiteboard"
        },
        "enable_chalkboard": true
      },

Click save and close/re-open the notebook. With chalkboard running, in class annotations can be saved using the `\` hotkey as a .json. To load those annotations (for decktape, or after re-opening the notebook) use the `"src": "saved_output.json",` parameter.  

decktape is useful for compiling the slides into a .pdf presentation. Chalkboard annotations can be included in the pdf output if a src json is include in the notebook metadata. These are the steps I use for decktape pdf compilation from RISE enabled notebook:  

-   First, boot up a jupyter notebook server: `jupyter notebook`
-   While that server is running, in another terminal type: `decktape -s '1920x1080' rise http://localhost:8888/notebooks/path/to/notebook/notebook.ipynb?token=16c257cbc57b3d6937825f921089c5e4fdba89c2d838ed1a outputfile.pdf` (the token corresponds to the jupyter server you booted up during step 1. If you are unsure of the path, navigate to the relevant file in your browser on the running server, and then copy that path for the decktape command.) If there are issues with the pdf cutting off content that was visible during your presentation, make sure the -s argument is used and pass it the resolution of your screen.
