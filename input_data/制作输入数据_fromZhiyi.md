因为我用的CLM5.0

1. 我首先是产生mapping文件,一共做了三次，不同就在不同组件（陆地啊海洋啊海冰啊那些）的分辨率；

```s
#--------------------------------------------------------------------
CESMroot="/mnt/iusers01/fatpou01/sees01/s29826zs/CESM/my_cesm_sandbox_2.2.0"
VRrepoPath="/mnt/iusers01/fatpou01/sees01/s29826zs/scratch/MUSICARE"
VRgridName="ne0np4.UK_ne30x16"
VRscripName="UK_ne30x16_np4_SCRIP.nc"
cdate="240625"

##mapDir="${CESMroot}/cime/tools/mapping/gen_mapping_files"
mapDir="${PWD}/../mapping/gen_mapping_files"

#--------------------------------------------------------------------
# For Typical runs the ocn/lnd/atm are on the same grid. 
# The specified grids are either VRM scrip files or grids 
# specified from the CESM inputdata repository. When 
# the same grid is specified for 2 components, map creation 
# is skipped.
#--------------------------------------------------------------------
atmName=${VRgridName}
atmGridName="${VRrepoPath}/${atmName}/grids/${VRscripName}"
lndName=${VRgridName}
lndGridName="${VRrepoPath}/${lndName}/grids/${VRscripName}"
### lndName="1x1"
### lndGridName="/glade/p/cesm/cseg/inputdata/share/scripgrids/1x1d.nc"
ocnName=${VRgridName}
ocnGridName="${VRrepoPath}/${ocnName}/grids/${VRscripName}"
### ocnName="gx1v7"
### ocnGridName="/glade/p/cesm/cseg/inputdata/share/scripgrids/gx1v7_151008.nc"
rofName="r05"
rofGridName="/mnt/iusers01/fatpou01/sees01/s29826zs/scratch/Projects/inputdata/lnd/clm2/mappingdata/grids/SCRIPgrid_0.5x0.5_nomask_c110308.nc"
glcName="gland4km"
glcGridName="/mnt/iusers01/fatpou01/sees01/s29826zs/scratch/Projects/inputdata/glc/cism/griddata/SCRIPgrid_greenland_4km_epsg3413_c170414.nc"
wavName="ww3a"
wavGridName="/mnt/iusers01/fatpou01/sees01/s29826zs/scratch/Projects/inputdata/share/scripgrids/ww3a_120222.nc"

#wgtFileDir="${VRrepoPath}/${VRgridName}/maps.${cdate}/"
wgtFileDir="${VRrepoPath}/${VRgridName}/maps/"
mkdir -p $wgtFileDir

echo " "
echo "==============================================================="
echo " Create mapping weight files for atm/ocn/lnd/rof/glc/wav grids:"
echo " "
echo " Atmosphere:"
echo " -----------"
echo " Grid Name= "${atmName}
echo " Grid File= "${atmGridName}
echo " "
echo " Land:"
echo " -----------"
echo " Grid Name= "${lndName}
echo " Grid File= "${lndGridName}
echo " "
echo " Ocean:"
echo " -----------"
echo " Grid Name= "${ocnName}
echo " Grid File= "${ocnGridName}
echo " "
echo " ROF Grid:"
echo " -----------"
echo " Grid Name= "${rofName}
echo " Grid File= "${rofGridName}
echo " "
echo " GLC Grid:"
echo " -----------"
echo " Grid Name= "${glcName}
echo " Grid File= "${glcGridName}
echo " "
echo " WAV Grid:"
echo " -----------"
echo " Grid Name= "${wavName}
echo " Grid File= "${wavGridName}
echo " "
echo " Mapping files will be created in the directory:"
echo " ----------------------------------------------- "
echo ${wgtFileDir}
echo " "
echo "==============================================================="
echo " "


#-------------------------------------
# It's all automatic from here on out.
#-------------------------------------
cd ${wgtFileDir}

if [ "$atmName" != "$lndName" ]; then
  if [ "$atmName" != "$ocnName" ]; then
    echo "Generating ATM <-> [LND,OCN,ROF,GLC] maps..... "

    ${mapDir}/gen_cesm_maps.sh -fatm ${atmGridName} -natm ${atmName} -flnd ${lndGridName} -nlnd ${lndName} -focn ${ocnGridName} -nocn ${ocnName} -frtm ${rofGridName} -nrtm ${rofName} -fglc ${glcGridName} -nglc ${glcName} -idate ${cdate}

  else
    echo "Generating ATM <-> [LND,ROF,GLC] maps..... "
    ${mapDir}/gen_cesm_maps.sh -fatm ${atmGridName} -natm ${atmName} -flnd ${lndGridName} -nlnd ${lndName} -frtm ${rofGridName} -nrtm ${rofName} -fglc ${glcGridName} -nglc ${glcName} -idate ${cdate}

  fi
else
  if [ "$atmName" != "$ocnName" ]; then
    echo "Generating ATM <-> [OCN,ROF,GLC] maps..... "
    ${mapDir}/gen_cesm_maps.sh -fatm ${atmGridName} -natm ${atmName} -focn ${ocnGridName} -nocn ${ocnName} -frtm ${rofGridName} -nrtm ${rofName} -fglc ${glcGridName} -nglc ${glcName} -idate ${cdate}

  else
    echo "Generating ATM <-> [ROF,GLC] maps..... "
    ${mapDir}/gen_cesm_maps.sh -fatm ${atmGridName} -natm ${atmName} -frtm ${rofGridName} -nrtm ${rofName} -fglc ${glcGridName} -nglc ${glcName} -idate ${cdate}
  fi
fi

############################# WAV <-> ATM ########################################

if [ "$wavName" != "$atmName" ]; then
  echo "Generating ATM <-> WAV maps..... "

  interp_method="blin"
  ${mapDir}/gen_ESMF_mapping_file/create_ESMF_map.sh -fsrc ${atmGridName} -nsrc ${atmName} -fdst ${wavGridName} -ndst ${wavName} -map ${interp_method} -idate ${cdate}

  interp_method="blin"
  ${mapDir}/gen_ESMF_mapping_file/create_ESMF_map.sh -fsrc ${wavGridName} -nsrc ${wavName} -fdst ${atmGridName} -ndst ${atmName} -map ${interp_method} -idate ${cdate}

fi

```

2. 然后是domain的文件，一共先是gx1v7，然后是tx0.1v2这两种分辨率

```s
CESMroot="/mnt/iusers01/fatpou01/sees01/s29826zs/CESM/my_cesm_sandbox_2.2.0"

# Set the path for the VRM files, the name of the grid as used 
# by CESM, and the path to the VRM SCRIP file. 
#---------------------------------------------------------------
VRrepoPath="/mnt/iusers01/fatpou01/sees01/s29826zs/scratch/MUSICARE"
VRgridName="ne0np4.UK_ne30x16"
VRscripFile="${VRrepoPath}/${VRgridName}/grids/UK_ne30x16_np4_SCRIP.nc"
cdate="240625"

# Path to CIME mapping tools
# Note: write access to this directory is necessary, so either 
# copy the exec or checkout and build in your own dir
#-----------------------------------------------------------
PATH_TO_MAPPING="${CESMroot}/cime/tools/mapping/"

# Set OCN grid to use.
# May need to change these settings, but safe to use t12 for mask 
# for anything >ne30
#----------------------------------------------------------------
#ocnName="tx0.1v2"
#ocnGridName="/glade/p/cesm/cseg/inputdata/share/scripgrids/tx0.1v2_090127.nc"
ocnName="gx1v7"
ocnGridName="/mnt/iusers01/fatpou01/sees01/s29826zs/scratch/Projects/inputdata/share/scripgrids/gx1v7_151008.nc"

# Set the map file needed
#-------------------------------------------
wgtFileDir="${VRrepoPath}/${VRgridName}/maps"
CurDate=`date +%y%m%d`
aaveMap="${wgtFileDir}/map_${ocnName}_TO_${VRgridName}_aave.${cdate}.nc"
echo ${aaveMap}


#----------------------------------------------------------------------
# CREATE MAPPING FILE
#----------------------------------------------------------------------
cd $PATH_TO_MAPPING/gen_domain_files

# do ATM2OCN_FMAPNAME (aave)
#-----------------------------
#ESMFBIN_PATH="/glade/u/apps/ch/opt/esmf/7.0.0-ncdfio-mpi/intel/17.0.1/bin/binO/Linux.intel.64.mpi.default"
#ESMFBIN_PATH="/glade/u/apps/ch/opt/esmf/7.0.0-ncdfio/intel/17.0.1/bin/bing/Linux.intel.64.mpiuni.default"
interp_method="conserve"   # bilinear, patch, conserve
ESMF_RegridWeightGen --ignore_unmapped -m ${interp_method} -w ${aaveMap} -s ${ocnGridName} -d ${VRscripFile}
##${ESMFBIN_PATH}/ESMF_RegridWeightGen --ignore_unmapped -m ${interp_method} -w ${aaveMap} -s ${ocnGridName} -d ${VRscripFile}

#----------------------------------------------------------------------
# CREATE DOMAIN FILES
#----------------------------------------------------------------------

set +e
./gen_domain -m ${aaveMap} -o gx1v7 -l ${VRgridName}

#----------------------------------------------------------------------
# MOVING FILES + CLEANUP 
#----------------------------------------------------------------------
# Move domain files to VRrepoPath dir
mkdir -p ${VRrepoPath}/${VRgridName}/domains
## mv domain*${VRgridName}*${cdate}*nc ${VRrepoPath}/${VRgridName}/domains
mv domain.lnd.${VRgridName}_${ocnName}.${CurDate}.nc ${VRrepoPath}/${VRgridName}/domains/domain.lnd.${VRgridName}_${ocnName}.${cdate}.nc
mv domain.ocn.${VRgridName}_${ocnName}.${CurDate}.nc ${VRrepoPath}/${VRgridName}/domains/domain.ocn.${VRgridName}_${ocnName}.${cdate}.nc


```

3. 最后是产生那个CLMsrfdata （1850-2000年的）

```s
cdate=`date +%y%m%d` # Get data in YYMMDD format

# Create TMPDIR
### TMPDIR=${TMPDIRBASE}/tmp.clmsurfdata.${cdate}/
TMPDIR=${TMPDIRBASE}/maps_clm/
mkdir -p ${TMPDIR}

# Use for CESM2.0xx
MKMAPDATADIR=${CESMROOT}/components/clm/tools/mkmapdata/

cd ${TMPDIR}
regrid_num_proc=15
#REL2.2 time env ESMFBIN_PATH=${ESMFBIN_PATH} REGRID_PROC=$regrid_num_proc ${MKMAPDATADIR}/mkmapdata.sh -b -v --gridfile ${VRscripFile} --res ${VRgridName} --gridtype global
time env CSMDATA=${CSMDATA_PATH} REGRID_PROC=$regrid_num_proc ${MKMAPDATADIR}/mkmapdata.sh -b -v --gridfile ${VRscripFile} --res ${VRgridName} --gridtype global

cd ${CESMROOT}/components/clm/tools/mksurfdata_map/

if ($DO_SP_ONLY); then
  CROPSTRING="-no-crop"
else
  CROPSTRING=""
fi
./mksurfdata.pl -years 1850-2000,1850,2000 ${CROPSTRING} -res usrspec -usr_gname ${VRgridName} -usr_gdate ${cdate} -usr_mapdir ${TMPDIR} -l ${CSMDATA_PATH} -exedir ${CESMROOT}/components/clm/tools/mksurfdata_map

## Move the surface datasets
mkdir -p ${VRrepoPath}/${VRgridName}/clm_surfdata_${CLMVERSION}
mv landuse*${VRgridName}*nc surfdata_${VRgridName}_*.nc ${VRrepoPath}/${VRgridName}/clm_surfdata_${CLMVERSION}

cd ${VRrepoPath}/${VRgridName}/clm_surfdata_${CLMVERSION}
mv landuse.timeseries_${VRgridName}_hist_16pfts_Irrig_CMIP6_simyr1850-2015_c${cdate}.nc  landuse.timeseries_${VRgridName}_hist_16pfts_Irrig_CMIP6_simyr1850-2015_c${MYcdate}.nc
mv surfdata_${VRgridName}_hist_16pfts_Irrig_CMIP6_simyr1850_c${cdate}.nc surfdata_${VRgridName}_hist_16pfts_Irrig_CMIP6_simyr1850_c${MYcdate}.nc 
mv surfdata_${VRgridName}_hist_16pfts_Irrig_CMIP6_simyr2000_c${cdate}.nc surfdata_${VRgridName}_hist_16pfts_Irrig_CMIP6_simyr2000_c${MYcdate}.nc 
```

