<template>
  <b-container>
    <b-row>
      <b-col>
        <div class="pt-4 pb-2">
          <p>
            Upload your image to see some of the useful metadata it contains,
            all processing is done directly in the browser so the image is not transmitted or stored.
          </p>

          <p class="mb-0">
            Exif data may be found in compressed JPEG images and may contain various pieces of information such as;
            GPS data, lens information, camera settings used, editing software, etc. See more information
            <a
              href="https://en.wikipedia.org/wiki/Exif"
            >here</a>.
          </p>
        </div>
      </b-col>
    </b-row>

    <hr />

    <!-- Loader -->
    <b-row>
      <b-col>
        <div class="text-center">
          <div v-if="loading">
            <b-spinner class="spinner m-5" variant="light" label="Loading..."></b-spinner>
          </div>
        </div>
      </b-col>
    </b-row>

    <!-- File Input -->
    <b-row>
      <b-col md="8" offset-md="2">
        <b-form-file
          v-if="!loading && !Boolean(image)"
          placeholder="Choose a file..."
          drop-placeholder="Drop file here..."
          @change="loadFromFileInput"
          accept="image/jpeg"
          :state="fileInputState"
        >

        </b-form-file>
      </b-col>
    </b-row>

    <!-- URL Input -->
    <b-row>
      <b-col md="8" offset-md="2">
        <b-input-group class="mt-3" v-if="!loading && !Boolean(image)">
          <b-form-input :state="urlInputState" v-model="url" placeholder="URL to image"></b-form-input>
          <b-input-group-append>
            <b-button @click="loadFromURL" class="submit-button">Submit</b-button>
          </b-input-group-append>
        </b-input-group>
      </b-col>
    </b-row>

    <!-- Image -->
    <b-row>
      <b-col>
        <b-img v-if="Boolean(image)" class="m-3" fluid center :src="image"></b-img>
      </b-col>
    </b-row>

    <hr v-if="Boolean(image)" />

    <!-- Metadata Information -->
    <div class="meta-information text-center" v-if="metadata !== null">
      <b-row>
        <b-col>
          <h3 class="text-center">Exif Data</h3>
        </b-col>
      </b-row>

      <b-row>
        <b-col cols="6">
          <h5>
            <u>File Info</u>
          </h5>

          <div v-if="metadata.DateTimeOriginal != undefined" class="group">
            <div class="key">Photo taken:</div>
            <div class="value">{{metadata.DateTimeOriginal}}</div>
          </div>

          <div v-if="metadata.ModifyDate != undefined" class="group">
            <div class="key">Modified Date:</div>
            <div class="value">{{metadata.ModifyDate}}</div>
          </div>

          <div v-if="metadata.Software != undefined" class="group">
            <div class="key">Software:</div>
            <div class="value">{{metadata.Software}}</div>
          </div>

          <div v-if="metadata.Artist != undefined" class="group">
            <div class="key">Artist:</div>
            <div class="value">{{metadata.Artist}}</div>
          </div>

          <div v-if="metadata.Copyright != undefined" class="group">
            <div class="key">Copyright:</div>
            <div class="value">{{metadata.Copyright}}</div>
          </div>
        </b-col>

        <b-col cols="6">
          <h5>
            <u>Settings and Exposure</u>
          </h5>

          <div v-if="metadata.ExposureTime != undefined" class="group">
            <div class="key">Shutter Speed:</div>
            <div class="value">{{metadata.ExposureTime}} sec</div>
          </div>

          <div v-if="metadata.FNumber != undefined" class="group">
            <div class="key">Aperture:</div>
            <div class="value">{{metadata.FNumber}}F</div>
          </div>

          <div v-if="metadata.ISO != undefined" class="group">
            <div class="key">ISO:</div>
            <div class="value">{{metadata.ISO}}</div>
          </div>

          <div v-if="metadata.FocalLength != undefined" class="group">
            <div class="key">Focal Length:</div>
            <div class="value">{{metadata.FocalLength}}mm</div>
          </div>

          <div v-if="metadata.FocalLengthIn35mmFormat != undefined" class="group">
            <div class="key">Full Frame Focal Length:</div>
            <div class="value">{{metadata.FocalLengthIn35mmFormat}}mm</div>
          </div>

          <div v-if="metadata.ExposureMode != undefined" class="group">
            <div class="key">Exposure Mode:</div>
            <div class="value">{{metadata.ExposureMode}}</div>
          </div>

          <div v-if="metadata.ExposureProgram != undefined" class="group">
            <div class="key">Exposure Program:</div>
            <div class="value">{{metadata.ExposureProgram}}</div>
          </div>

          <div v-if="metadata.ExposureCompensation != undefined" class="group">
            <div class="key">Exposure Compensation:</div>
            <div class="value">{{metadata.ExposureCompensation}}</div>
          </div>
        </b-col>

        <b-col cols="6">
          <h5>
            <u>Camera &amp; Lens</u>
          </h5>

          <div v-if="metadata.OwnerName != undefined" class="group">
            <div class="key">Body Owner Name:</div>
            <div class="value">{{metadata.OwnerName}}</div>
          </div>

          <div v-if="metadata.Make != undefined" class="group">
            <div class="key">Camera Make:</div>
            <div class="value">{{metadata.Make}}</div>
          </div>

          <div v-if="metadata.Model != undefined" class="group">
            <div class="key">Camera Model:</div>
            <div class="value">{{metadata.Model}}</div>
          </div>

          <div v-if="metadata.LensMake != undefined" class="group">
            <div class="key">Lens Make:</div>
            <div class="value">{{metadata.LensMake}}</div>
          </div>

          <div v-if="metadata.LensModel != undefined" class="group">
            <div class="key">Lens Model:</div>
            <div class="value">{{metadata.LensModel}}</div>
          </div>

          <div v-if="metadata.SerialNumber != undefined" class="group">
            <div class="key">Body Serial Number:</div>
            <div class="value">{{metadata.SerialNumber}}</div>
          </div>

          <div v-if="metadata.LensSerialNumber != undefined" class="group">
            <div class="key">Lens Serial Number:</div>
            <div class="value">{{metadata.LensSerialNumber}}</div>
          </div>
        </b-col>

        <b-col cols="6" v-if="metadata.GPSInfo != undefined">
          <h5>
            <u>GPS Info</u>
          </h5>

          <div v-if="metadata.GPSVersionID != undefined" class="group">
            <div class="key">Version ID:</div>
            <div class="value">{{metadata.GPSVersionID}}</div>
          </div>

          <div
            v-if="metadata.GPSLatitude != undefined && metadata.GPSLatitudeRef != undefined"
            class="group"
          >
            <div class="key">Latitude:</div>
            <div class="value">{{metadata.GPSLatitude}} {{metadata.GPSLatitudeRef}}</div>
          </div>

          <div
            v-if="metadata.GPSLongitude != undefined && metadata.GPSLongitudeRef != undefined"
            class="group"
          >
            <div class="key">Longitude:</div>
            <div class="value">{{metadata.GPSLongitude}} {{metadata.GPSLongitudeRef}}</div>
          </div>

          <a
            v-if="metadata.GPSLatitude != undefined && metadata.GPSLatitudeRef != undefined
            && metadata.GPSLongitude != undefined && metadata.GPSLongitudeRef != undefined"
            :href="'https://www.google.com/maps/search/?api=1&query=' + metadata.GPSLatitude + ' ' + metadata.GPSLatitudeRef + ', ' + metadata.GPSLongitude + ' ' + metadata.GPSLongitudeRef"
            onmousedown="event.preventDefault()"
            target="_blank"
            class="btn btn-default my-2"
          >
            <i class="fas fa-map-marker-alt fa-fw"></i>
            <span class="button-text">view on map</span>
          </a>

          <div v-if="metadata.GPSAltitudeRef != undefined" class="group">
            <div class="key">Altitude:</div>
            <div class="value">{{metadata.GPSAltitudeRef}}</div>
          </div>
        </b-col>
      </b-row>
    </div>

    <b-row>
      <b-col>
        <div class="text-center pt-2">
          <b-btn v-if="Boolean(image)" @click="reset" size="lg" target="_blank" class="btn btn-default">
            <i class="fas fa-redo fa-fw mr-1"></i>
            <span class="button-text">reset</span>
          </b-btn>
        </div>
      </b-col>
    </b-row>

    <hr />

    <b-row>
      <b-col>
        <h5>Example Images</h5>
        <p>Some example photos taken by me. Clicking the buttons below will give you an idea how the image is displayed.</p>
      </b-col>
    </b-row>

    <b-row>
      <b-col>
        <b-btn @click="loadExample(1)" class="btn btn-default mr-2">
          <!-- <i class="fab fa-npm fa-fw"></i> -->
          <span class="button-text">Example 1 - Camera</span>
        </b-btn>
        <b-btn @click="loadExample(2)" class="btn btn-default mr-2">
          <!-- <i class="fab fa-npm fa-fw"></i> -->
          <span class="button-text">Example 2 - Drone</span>
        </b-btn>
        <b-btn @click="loadExample(3)" class="btn btn-default mr-2">
          <!-- <i class="fab fa-npm fa-fw"></i> -->
          <span class="button-text">Example 3 - Phone with GPS</span>
        </b-btn>
      </b-col>
    </b-row>
  </b-container>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import axios from 'axios'
import ImageMeta from 'image-meta-js'

@Component
export default class Meta extends Vue {
      debug = true;
      loading = false;
      metadata = null;
      file = null;
      url = '';
      image: string | ArrayBuffer | null = null;

      fileInputState: boolean | null = null;
      urlInputState: boolean | null = null;

      loadExample (n: string): void{
        const fileLocation = require(`@/assets/images/example-${n}.jpg`)
        this.imageURLToBlob(fileLocation, this.parseMeta)
      }

      loadFromURL (): void {
        if (this.url.length === 0) {
          this.urlInputState = false
          return
        }

        this.imageURLToBlob(this.url, this.parseMeta)
      }

      loadFromFileInput (event: Event): void{
        const file = (event.target as HTMLInputElement).files as FileList

        if (!file[0] || !file[0].type.match(/image.jpeg/)) {
          alert('Invalid file format')
          return
        }

        this.parseMeta(file[0])
      }

      parseMeta (file: Blob): void{
        this.loading = true
        const reader = new FileReader()

        reader.readAsArrayBuffer(file)
        reader.onload = () => {
        // Lots of type shuffling
          const arrayBuffer = reader.result as ArrayBufferLike

          const binary = new DataView(arrayBuffer)

          // Load the image into the library
          try {
            const meta = new ImageMeta(binary, this.debug)
            this.metadata = meta.data()
          } catch (e) {
          // Catch exceptions from the library
            this.loading = false
            alert(e)
            return
          }

          this.loadImagePreview(file)
        }
      }

      loadImagePreview (image: Blob): void{
        const imageReader = new FileReader()
        imageReader.readAsDataURL(image)

        imageReader.onload = () => {
          this.image = imageReader.result
          this.loading = false // Cancel loading icon
        }
      }

      reset (): void{
        this.metadata = null
        this.image = null
        this.file = null
        this.url = ''
        this.fileInputState = null
        this.urlInputState = null
      }

      imageURLToBlob (url: string, callback: (f: Blob) => void): void{
        axios
          .get(url, {
            responseType: 'blob'
          })
          .then(response => {
            callback(response.data)
          })
          .catch(err => {
            if (err.request.status === 404) {
              alert('404: Not found - please check the URL is correct and try again')
            } else {
              alert('There has been an error fetching the image')
            }
          })
      }
}
</script>

<style scoped lang="scss">
img {
  max-width: 60%;
}

.spinner {
  width: 6rem;
  height: 6rem;
}

u {
  text-underline-position: under;
}

.group {
  color: white;
  padding-bottom: 5px;
}

.key {
  font-weight: 200;
  font-size: 0.9rem;
  padding-right: 5px;
  display: inline-block;
}

.value {
  font-weight: 600;
  display: inline-block;
}

.meta-information [class*="col-"] {
  padding-top: 20px;
}

input::placeholder {
  color:#495057;
}

.btn-default {
  margin-top:5px;
  margin-bottom:5px;
}

.submit-button {
  color:#495057;
  background-color:#e9ecef;
}

.btn-secondary {
  border-color:rgb(206, 212, 218);
}

</style>
