-tidy-data-
===========
# Load activity_labels.txt
# carga activity_labels.txt
archivo_activity_labels <- read.table("./UCI HAR Dataset/activity_labels.txt")[,2]

# load column names
# carga nombres de columna
caracteristicas <- read.table("./UCI HAR Dataset/features.txt")[,2]

# Extracting measurements mean and standard deviation for each measurement
# Extraer las mediciones de la desviación media y estándar para cada medición
extrae_caracteristicas <- grepl("mean|std", caracteristicas)

# Load x_test.txt, y_test.txt y subject_test.txt
# carga x_test.txt, y_test.txt y subject_test.txt
archivo_x_test <- read.table("./UCI HAR Dataset/test/X_test.txt")
archivo_y_test <- read.table("./UCI HAR Dataset/test/y_test.txt")
archivo_subject_test <- read.table("./UCI HAR Dataset/test/subject_test.txt")
names(archivo_x_test) = caracteristicas

# Extracting measurements mean and standard deviation for each measurement
# Extraer las mediciones de la desviación media y estándar para cada medición
archivo_x_test = archivo_x_test[,extrae_caracteristicas]

# load labels
# carga etiquetas
archivo_y_test[,2] = archivo_activity_labels[archivo_y_test[,1]]
names(archivo_y_test) = c("Activity_ID", "Activity_Label")
names(archivo_subject_test) = "subject"

# bind data
# enlazar datos
prueba_data <- cbind(as.data.table(archivo_subject_test), archivo_y_test, archivo_x_test)

# Load X_train.txt, y_train.txt y subject_train.txt
# carga X_train.txt, y_train.txt y subject_train.txt
archivo_x_train <- read.table("./UCI HAR Dataset/train/X_train.txt")
archivo_y_train <- read.table("./UCI HAR Dataset/train/y_train.txt")
archivo_subject_train <- read.table("./UCI HAR Dataset/train/subject_train.txt")
names(archivo_x_train) = caracteristicas

# Extracting measurements mean and standard deviation for each measurement
# Extraer las mediciones de la desviación media y estándar para cada medición
archivo_x_train = archivo_x_train[,extrae_caracteristicas]

# Load data
# carga datos
archivo_y_train[,2] = archivo_activity_labels[archivo_y_train[,1]]
names(archivo_y_train) = c("Activity_ID", "Activity_Label")
names(archivo_subject_train) = "subject"

# bind data
# enlazar datos
train_data <- cbind(as.data.table(archivo_subject_train), archivo_y_train, archivo_x_train)

# combine data
# combinar datos
data = rbind(prueba_data, train_data)
codigo_lbl   = c("subject", "Activity_ID", "Activity_Label")
str_lbls = setdiff(colnames(data), codigo_lbl)
drt_str      = melt(data, id = codigo_lbl, measure.vars = str_lbls)

# applying function average to dataset using dcast
# aplicar funcion de media al dataset utilizando dcast
archivo_tidy_data   = dcast(drt_str, subject + Activity_Label ~ variable, mean)

# create file archivo_tidy_data.txt
# crear archivo archivo_tidy_data.txt
write.table(archivo_tidy_data, file = "./archivo_tidy_data.txt")

course project
