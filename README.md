# generate-artifact-tag
Outputs a string value for use as an artifact tag, intended for a Docker image or directory name. 
The output is dependent on whether this is a main build or a release creation event.

## Usage
```
jobs:
  deploy:
    environment:
      name: ${{ inputs.ENV_NAME }}

    steps:
      - name: Get artifact tag
        id: generate-artifact-tag
        uses: paul-ylz/generate-artifact-tag@v0.0.2

      - name: Set image tag variable
        run: |
          set -eux
          echo "IMAGE_TAG=${{ steps.generate-artifact-tag.outputs.tag }}" >> $GITHUB_ENV
          echo $GITHUB_ENV

      - name: Render Amazon ECS task definition
        uses: aws-actions/amazon-ecs-render-task-definition@v1
        with:
          task-definition: build/${{ inputs.ENV_NAME }}/taskdef.json
          container-name:  mycontainer-${{ inputs.ENV_NAME }}
          image: 00000000000.dkr.ecr.ap-southeast-1.amazonaws.com/my-ecr-repo:${{ env.IMAGE_TAG }}
```
