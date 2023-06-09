name: Burpsuite DAST Scan
on:
  workflow_dispatch:
    inputs:
      TARGET_URL:
        type: string
        required: true
        description: " Target URL for Scanning "
      Slack:
        type: choice
        description: " Upload Report To Slack " 
        options:
          - Yes
          - No
        required: true
      Channel:
        type: string
        required: false
jobs:
  Run-Scanner:
    name: Dastardly
    runs-on: ubuntu-latest
    
    steps:
      - name: Check Inputs
        run: |
            echo "${{ inputs.SLACK }}"
        
      - name: Running Dast Scan 
        id: Scan
        continue-on-error: true
        uses: PortSwigger/dastardly-github-action@v1.0.0
        with:
          target-url: ${{ inputs.TARGET_URL }}
      
      - name: Install Some dependencies
        run: pip install junit2html
      
      - name: XMl to HTMl
        run: |
          cd ${GITHUB_WORKSPACE}
          junit2html dastardly-report.xml dastardly-report.html
      
      - name: Upload Artifact
        uses: actions/upload-artifact@v3.1.2 
        with:
          name: Dastrdly-Report
          path: "**/dastardly-report.html"
      
        
      - name: Send Report to Slack
        if: ${{ inputs.SLACK == 'true' }} 
        uses: adrey/slack-file-upload-action@1.0.5
        with:
          token: ${{ secrets.SLACK_BOT_TOKEN }}
          path: "${{ github.workspace }}/dastardly-report.html"
          channel: ${{ inputs.Channel }}
